Aiida
=====

`Aiida <https://aiida.readthedocs.io/projects/aiida-core/en/latest/>`_ is a great tool for managing computational tasks. And it can also be used in high-thoughput computing.

I started learn how to use Aiida in November, 2020, and it suddenly release my burden on how to manage computational tasks. Before Aiida, all the management are done by using folders, which can be really inefficient and chaotic, but with Aiida, I can put everything in one jupyter notebook, then manage all the work, the connection to the supercomputer and also the database in one place.

I have written two tutorial articles about how to install and configure Aiida on `Ubuntu <https://zhuanlan.zhihu.com/p/295175866>`_ and `macOS <https://zhuanlan.zhihu.com/p/300490416>`_, since I have a Mac, so on macOS could be a better choice since the virtual machine is not as efficient as the main OS.

When we try to learn Aiida or other programming frameworks, the first thing is always: get a demo running. Because you can learn a lot of things by *trial-and-error*

Some common mistakes in using Aiida
-----------------------------------

In here are some mistakes that I have been experienced:

aiida.common.exceptions.MissingEntryPointError: Entry point 'quantumespresso.pw' not found in group 'aiida.parsers'.
********************************************************************************************************************

You need to check these things below:

* Whether you have the parser in :code:`aiida.parsers`, you can check that by typing: :code:`verdi plugin list aiida.parsers`, if :code:`quantumespresso.pw` is in, then you are OK for this check. If not, then you need to type :code:`reentry scan` and re-check again.

* If the first step is checked, then you should restart the daemon: :code:`verdi daemon restart --reset`, because the daemon may not have the information on the parser.

For detailed information, please see `this <https://groups.google.com/g/aiidausers/c/-zvnONEVIN0>`_

Write aiida plugins
-------------------

In Aiida, I think the most important and most interesting concept is "plugins", which can extend the capability of aiida-core, also give the user more freedom to create their own workflow in order to make the work more engaging and interesting (offload all the boring stuff).

But writing aiida plugin is not that straight forward, I'm learning that right now. But reading without thinking is bad, and what better way to think than just writing about one's thought, that's why I'm writing all my thoughts in here in order to help me understand more about the structure of aiida-core and how everything connects together. Let's begin!

If we take a look at the :code:`aiida-quantumespresso`, we will see that the structure is:

.. figure:: ./aiida_image/aiida_qe.png
    :width: 600
    :align: center

    Screenshot of the structure of aiida_quantumespresso plugins

the most important folders are: :code:`calculations`, :code:`parsers` and :code:`workflows`, others are supplementary.

So if we want to write the aiida plugins by ourselves, we need to be able to:

* Write the calculations for a specific external program (e.g. VASP, CPMD, CP2K, Quantum Espresso, etc.)
* Write the parser for the results
* Write the workflows and workchains (workchain is future, so our main effort will be on writing workchain, but we will also talk about how to write the workflow and what's the difference between them. In Aiida's official website they have a really good explanation, but I want to have my own version of that.)

Write the calculation
*********************

We should start with how to write the calculation. In order to do that we need to know what a calculation actually means in Aiida. So in aiida, The father class of all calculations is this class: :code:`aiida.engine.CalcJob`, so in order to write a calculation, we need to inherit from this class, something like:

.. code-block:: Python

    from aiida.engine import CalcJob

    class somethingCalculation(CalcJob):

In this :code:`CalcJob` class, we need to define two functions: (1) :code:`define()` (2) :code:`prepare_for_submission()`. The first function (:code:`define()`) is used for declaring all the inputs, outputs, error messages that can happen during the calculation, and the second function (:code:`prepare_for_submission()`) is used for preparing all the input files and submit to the queue, managed by :code:`daemon` (we will talk about that later).

Ok, now let's try to write the :code:`define()` function, in here I give an example (from aiida official website), and try to explain it line by line, because that's how you learn programming, but reading other people's code and try to explain to others like you wrote them (even though you are not). That's `Feynman technique <https://medium.com/taking-note/learning-from-the-feynman-technique-5373014ad230>`_.

Our :code:`define()` function is like in below:

.. code-block:: Python

    from aiida import orm
    from aiida.engine import CalcJob, CalcJobProcessSpec

    class hzdCalculation(CalcJob):
        @classmethod
        def define(cls, spec:CalcJobProcessSpec):
            super().define(spec)

            # input parameter example
            spec.input('x', valid_type=(orm.Int, orm.Float), required=True, default=1, help='This is our first variable')

            # output parameter example
            spec.output('output', validtype=(orm.Int, orm.Float), help='This is the output of this calculation.')

            # output some error messages
            spec.exit_code(666, 'EVIL_IS_COMING', message='This is my first error message.')

            # set the default parser
            spec.inputs['metadata']['options']['parser_name'].default = 'parser_name'

This is a super simple way to define the :code:`define()` function. We should look at it line by line and try to explain everything that is happening right now:

* :code:`from aiida import orm`

:code:`orm` is the module in python which contains all aiida-specific types, you can only keep provenance in aiida if you use the type that they have provided, and there are lots of them, you can see that in the figure below:

.. figure:: ./aiida_image/aiida_orm.png
    :width: 600
    :align: center

    types in aiida.orm module

There are some usual types (e.g. int, float) and other special types (e.g. FolderData, StructureData, etc.), but if you want to keep provenance in your calculation (which is recommended by the Aiida), you should use the types provided by aiida.

* :code:`from aiida.engine import CalcJob, CalcJobProcessSpec`

We know that the :code:`CalcJob` is the father class of the our calculation class, but we need to understand more about the :code:`CalcJobProcessSpec`. This is the class for :code:`spec` in our code, if you want to learn more about it, please refer to this `website <https://aiida.readthedocs.io/projects/aiida-core/en/latest/reference/apidoc/aiida.engine.processes.html#aiida.engine.processes.process_spec.CalcJobProcessSpec>`_

* :code:`class hzdCalculation(CalcJob):`

This line is really simple, we define a new class called :code:`hzdCalculation`, and this class is inherited from :code:`CalcJob`.

* :code:`@classmethod` is the decorator of a class function. A class function is only defined in the class, the object generated by the class cannot access them. If you want to learn more about decorator, please refer to my tutorial on python in `computerlanguages` section.

* :code:`def define(cls, spec:CalcJobProcessSpec):`

This line define the function :code:`define()`, the :code:`cls` is common practice in python while defining a class function. The type of the :code:`spec` is :code:`CalcJobProcessSpec`, basically you can think about it as an dictionary. Well, a class is just like a type, but with more functionalities (methods in the class). Thats the essence of OOP, it is just a way of organizing your code. For simplicity we can just write: :code:`define(cls, spec)`.

* :code:`super().define(spec)`

The :code:`super()` means the father class (in this case it is the :code:`CalcJob` class). Then we use the class method :code:`define(spec)`.

* :code:`spec.input('x', valid_type=(orm.Int, orm.Float), help='This is our first variable')`

This is the declaration of the input parameters in the hzdCalculation. :code:`x` is the name of this input, :code:`valid_type` is the valid type of this variable, and :code:`help` is the meaning of this variable. If :code:`required` is not mentioned, then this variable is required, otherwise we should set :code:`required=False`, which means this parameter is not required.

You can define lots of input parameters by using this statement. In the :code:`CalcJob` class, if you look at the `source code <https://aiida.readthedocs.io/projects/aiida-core/en/latest/_modules/aiida/engine/processes/calcjobs/calcjob.html#CalcJob.define>`_), there are many variables have been pre-defined:

.. figure:: ./aiida_image/aiida_calcjob.png
    :width: 600
    :align: center

    All the pre-defined parameters that we could access in the calculation class.

We can see that some variable is named: :code:`metadata.options.resources`, that means that we can add hierarchy to the input parameters, for example when we want to build a :code:`PwCalculation` job, then sometimes we will write something like this:

.. code-block:: Python

    codename = 'hzd_code'
    code = Code.get_from_string(codename)
    builder = code.get_builder()

    builder.structure = structure
    builder.metadata.options.resources = {'num_machines': 2}

So the input you define in the :code:`hzdCalculation` class will be given by the :code:`builder` when you try to use it, understand this connection is very important.

The output parameters and the error message is almost the same as the input parameters, so we do not need to explain them more. As for the error message, Aiida has its own rules, which you can see it in the website.

Now we have done writing the :code:`define(cls, spec)` functions, now let's move on to the :code:`prepare_for_submission()` function.

A really simple example is given in below:

.. code-block:: Python

    def prepare_for_submission(self, folder: Folder) -> CalcInfo:
        with folder.open(self.options.input_filename, 'w', encoding='utf8') as handle:
            handle.write(something)

        codeinfo = CodeInfo()
        codeinfo.code_uuid = self.inputs.code.uuid
        codeinfo.stdin_name = self.options.input_filename
        codeinfo.stdout_name = self.options.output_filename

        calcinfo = CalcInfo()
        calcinfo.codes_info = [codeinfo]
        calcinfo.retrieve_list = [self.options.output_filename]

        return calcinfo

According to the `aiida website <https://aiida.readthedocs.io/projects/aiida-core/en/latest/howto/plugin_codes.html#how-to-plugin-codes-interfacing>`_, :code:`prepare_for_submission()` functions has two jobs: (1) creating the input files in the format that the external code expects (2) returning a :code:`CalcInfo` object that contains instructions for the Aiida engine about how the code should be run.

| The :code:`prepare_for_submission()` function is implemented from scratch, so there is no :code:`super()` method.

Write the Parser
****************

When we finished the simulation, we want to get as much information as possible for our analysis. So writing the parser for the output files is very important for Aiida. In this section we will talk about how to write a parser.

Like writing a calculation, writing a parser has to inherit from :code:`Parser` class, so the class declaration can be written as:

.. code-block:: Python

    class hzdParser(Parser):

And a more detailed (but simple) implementation is listed in below:

.. code-block:: Python

    class hzdParser(Parser):

        def parse(self, **kwargs):

            output_folder = self.retrived
            with output_folder.open(self.node.get_option('output_filename', 'r')) as handle:
                results = handle.read()

            self.out('output', someValue)

Add entry points
****************

For any Aiida plugins, we need to define the entry points, in this way Aiida will know which class it needs to call. Also entry point is a great way for simplifying the input. We need to define it in the :code:`setup.py` file in our package. And we should writing some like:

.. code-block:: Python

    from setuptools import setup

    setup(
        name='aiida-hzd',
        packaes=['aiida_hzd'],
        entry_points={
            'aiida.calculations': ['hzdCalculation = aiida_hzd.calculations:hzdCalculation'],
            'aiida.parsers': ['hzdParser = aiida_hzd.parsers:hzdParser'],
        }
    )

Then your plugin should be Ok, once you install the plugin, you should do the :code:`reentry scan` again to make sure that all the calculations, parsers, workflow and workchains are all implemented.

Write the workfunction and workchain
************************************

For now we know how to write the calculation and the parser, then we should move to more advanced stuff, meaning the workflow and workchain. The difference between workfunction and workchain is that: workfunction can only be :code:`run`, but workchain can be :code:`submit` to the daemon in order to run in the background (or on the remote server).

| workfunction and workchain are all workflows, they are just different implementation methods.
