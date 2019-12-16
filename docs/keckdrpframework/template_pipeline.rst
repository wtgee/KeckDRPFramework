Creating a pipeline using the template
======================================

In this example we will create a data reduction package called MyDRP.

The directory ``pipeline_template`` provides a simple starting point to create a data processing
pipeline.

Start by making a copy of the directory with all the included subdirectories::

  mkdir MyPipeline
  cd MyPipeline
  cp -r KeckDRPFramework/keckdrpframework/pipeline_template .

Setup.py
^^^^^^^^
You can now start editing the files in the new pipeline, starting with ``setup.py``. In this file,
it is important to edit the NAME, the description and the licence. Note that this file assumes that
any command line interface script will live in the ``scripts`` directory. Note also that the package name
is currently set to ``my_pipeline``. In the next step, we will rename this directory to be the actual
package name of your pipeline, so you might want to change this variable accordingly.

Create the main pipeline
^^^^^^^^^^^^^^^^^^^^^^^^

As a first step, rename the directory ``my_pipeline`` to be the name of the pipeline that you are creating::

  mv my_pipeline MyDRP

In the directory ``pipelines``, rename the ``template_pipeline.py``::

  cd MyDRP/pipelines
  mv template_pipeline.py MyDRP.py

You can now edit the file by completing the entries in the ``event_table``. A complete description of the
event table is provided in :doc:`events_actions.rst`. The ``import`` section of this file is made of two
parts: first we import the necessary framework modules such as::

  from keckdrpframework.pipelines.base_pipeline import BasePipeline

Then we import all the primitives that are defined in the ``primitives`` directory, and that will
ultimately provide the actual processing. In the simple case in which a single primitive is invoked,
a single entry in the event table is all that is needed.
Remember that the format for the event table is::

  event_name: (primitive_name, state, next event)

Which can be simplified to::

  event_name: (primitive_name, None, None)

if no state update is required and we don't need to trigger another event after the first.

Connecting the event to the code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Let's now turn to the primitives directory, and start by renaming the ``template_primitive.py`` file
to a suitable name::

  mv template_primitive.py mydrp_primitive.py

We can now edit the file to change the name of the primitive that is defined in the file. Change the name
``Template`` to the primitive_name that you have used in your event table. If the primitive that you are
defining here is called ``class DrpPrimitive:``, then your event table should look like this::

  event_table: {
     "mydrp_event": ("DrpPrimitive", None, None)
     }

At this point, we have created a basic pipeline, which only handles a single event. When the event is triggered,
the framework will run the code contained in the ``DrpPrimitive`` class.
See the :doc:`primitives.rst` documentation for a complete description of the primitives.







