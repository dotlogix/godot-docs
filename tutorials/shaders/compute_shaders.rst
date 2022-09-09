.. _doc_compute_shaders:

Using ComputeShaders
====================

Intro
-----

Unlike most shaders compute shaders don't have a defined functionality and thus are independant from the graphics pipeline.
You can think of them as blocks of code executed on the GPU for about any purpose you want.
The big benefit compared to code executed on a CPU is the high amount of parallelization that GPUs provide.

But because compute shaders are independant of the graphics pipeline you don't have any user defined inputs or outputs. Instead they make changes directly on the GPUs memory which you can read and write  using scripts.

How they work
-------------

Compute shaders can be though of as a mass of small computers called work groups.
Much like super computers they are aligned in rows and columns but also stacked on top of each other
essentially forming a 3D array of them.

When creating a compute shader we can specify the number of work groups we wish to use.
Keep in mind that these work groups are independant from each other and therefore should not be dependant on each other.

In each work group we have another 3D array of threads called invocations, but unlike work groups, invocations can communicate with each other.

So now lets work with a compute shader to see how it really works.

Creating a ComputeShader
-----------------------

To begin using ComputeShaders, create a new text file called "compute_example.glsl" GLSL is the shader language used in Godot so if you are familiar with normal shaders so the syntax below should look somehow familiar.

Let's take a look at this compute shader code:
.. code-block:: glsl

    #[compute]
	#version 450

	// Invocations in the x, y and z dimension
	layout(local_size_x = 4, local_size_y = 1, local_size_z = 1) in;

	// The binding we defined in out script
	layout(set = 0, binding = 0, std430) restrict buffer DataBuffer {
	  double data[];
	}
	data_buffer;

	// The code we want to execute in each invocation
	void main() {
		// gl_GlobalInvocationID.x uniquely identifies this invocation within the work group
		data_buffer.data[gl_GlobalInvocationID.x] *= 2.0;
	}


All this code does is taking an array of doubles, multiply each element by 2 and storing the result back in the original array.

To continue copy the code above in your newly created "compute_example.glsl" file.

Create a local RenderingDevice
------------------------------

To interact and execute a compute shader we need a script. So go ahead and create a new Script in the language of your choice and attach it to any Node in your scene.

Now to execute your shader we need a local :ref:`RenderingDevice <class_RenderingDevice>`.

You can create a rendering device using the :ref:`RenderingServer <class_RenderingServer>`:
.. tabs::
 .. code-tab:: gdscript GDScript

    # Create a local rendering device.
	var rd := RenderingServer.create_local_rendering_device()

 .. code-tab:: csharp

    // Create a local rendering device.
	var rd = RenderingServer.CreateLocalRenderingDevice();

You can then load your newly created shader file "compute_example.glsl" in your code and create a pre-compiled version of it using this code:
.. tabs::
 .. code-tab:: gdscript GDScript

	# Load GLSL shader
	var shader_file := load("res://compute_example.glsl")
	var shader_spirv: RDShaderSPIRV = shader_file.get_spirv()
	var shader := rd.shader_create_from_spirv(shader_spirv)
	
 .. code-tab:: csharp

	// Load GLSL shader
	var shaderFile = GD.Load<RDShaderFile>("res://compute_example.glsl");
	var shaderBytecode = shaderFile.GetSpirv();
	var shader = rd.ShaderCreateFromSpirv(shaderBytecode);


Provide inputs
--------------

As you might remember we want to pass an input array to our shader multiply each element by 2 and get the output.
To pass values to a compute shader you need a buffer in our case, because we are dealing with primitives we will use a storage buffer. A storage buffer takes an array of bytes and allows the CPU to transfer data to and from a GPU.

So let's create an array of doubles and create a storage buffer:
.. tabs::
 .. code-tab:: gdscript GDScript

	# Prepare our data. We use doubles in the shader, so we need 64 bit.
	var input := PackedFloat64Array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
	var input_bytes := input.to_byte_array()
	
	# Create a storage buffer that can hold our double values.
	# Each double has 8 byte (64 bit) so 10 x 8 = 80 bytes
	var buffer := rd.storage_buffer_create(input_bytes.size(), input_bytes)
	
 .. code-tab:: csharp

	// Prepare our data. We use doubles in the shader, so we need 64 bit.
	var input = new double[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	var inputBytes = new byte[input.Length * sizeof(double)];
	Buffer.BlockCopy(input, 0, inputBytes, 0, inputBytes.Length);
	
	// Create a storage buffer that can hold our double values.
	// Each double has 8 byte (64 bit) so 10 x 8 = 80 bytes
	var buffer = rd.StorageBufferCreate((uint)inputBytes.Length, inputBytes);

With the buffer in place we need to tell the rendering device to use this buffer.
To do that we need to create a uniform (as in normal shaders) and assign it to the rendering device.
.. tabs::
 .. code-tab:: gdscript GDScript

	# Create a uniform to assign the buffer to the rendering device
	var uniform := RDUniform.new()
	uniform.uniform_type = RenderingDevice.UNIFORM_TYPE_STORAGE_BUFFER
	uniform.binding = 0
	uniform.add_id(buffer)
	var uniform_set := rd.uniform_set_create([uniform], shader, 0)
	
 .. code-tab:: csharp
 
	// Create a uniform to assign the buffer to the rendering device
	var uniform = new RDUniform
	{
		UniformType = RenderingDevice.UniformType.StorageBuffer,
		Binding = 1
	};
	uniform.AddId(buffer);
	var uniformSet = rd.UniformSetCreate(new Array<RDUniform> { uniform }, shader, 0);


Defining a compute pipeline
---------------------------
Next we will create the compute pipeline for our shader to execute.

.. tabs::
 .. code-tab:: gdscript GDScript

	# Create a compute pipeline
	var pipeline := rd.compute_pipeline_create(shader)
	var compute_list := rd.compute_list_begin()
	rd.compute_list_bind_compute_pipeline(compute_list, pipeline)
	rd.compute_list_bind_uniform_set(compute_list, uniform_set, 0)
	rd.compute_list_dispatch(compute_list, 5, 1, 1)
	rd.compute_list_end()
	
 .. code-tab:: csharp
 
	// Create a compute pipeline
	var pipeline = rd.ComputePipelineCreate(shader);
	var computeList = rd.ComputeListBegin();
	rd.ComputeListBindComputePipeline(computeList, pipeline);
	rd.ComputeListBindUniformSet(computeList, uniformSet, 0);
	rd.ComputeListDispatch(computeList, xGroups: 5, yGroups: 1, zGroups: 1);
	rd.ComputeListEnd();

This code will execute the following steps: 
1. Create a new pipeline.
2. Begin a list of instructions for our GPU to execute.
3. Bind our compute list to our pipeline
4. Bind our buffer uniform to our pipeline
5. End the list of instructions


Execute a compute shader
------------------------

After all of this we are done, kind of. We still need to execute our pipeline everything so far was only definition not execution.

To execute your compute shader finally you just need to call these two methods below:

.. tabs::
 .. code-tab:: gdscript GDScript

	# Submit to GPU and wait for sync
	rd.submit()
	rd.sync()
	
 .. code-tab:: csharp
 
	// Submit to GPU and wait for sync
	rd.Submit();
	rd.Sync();


Retriving results
-----------------

Now we can retrieve our buffer again to get the output data and display it in our console.

.. tabs::
 .. code-tab:: gdscript GDScript

	# Read back the data from the buffer
	var output_bytes := rd.buffer_get_data(buffer)
	var output := output_bytes.to_float64_array()
	print(output)
	
 .. code-tab:: csharp

	// Read back the data from the buffers
	var outputBytes = rd.BufferGetData(outputBuffer);
	var output = new double[input.Length];
	Buffer.BlockCopy(outputBytes, 0, output, 0, outputBytes.Length);
	GD.Print(output)
