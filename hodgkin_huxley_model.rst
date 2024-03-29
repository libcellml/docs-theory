.. _hodgkin_huxley_model:

==================================================
The Hodgkin-Huxley model of nerve action potential
==================================================

Here we describe the first (and most famous) model of nerve fibre electrophysiology based on the membrane ion channels that we have discussed in the last two sections.
This is the work by Alan Hodgkin and Andrew Huxley in 1952 [#]_ that won them (together with John Eccles) the 1963 Noble prize in Physiology or Medicine for *"their discoveries concerning the ionic mechanisms involved in excitation and inhibition in
the peripheral and central portions of the nerve cell membrane"*.

.. contents::
    :local:


Theory: The cable equation
--------------------------
The *cable equation* was developed in 1890\ [#]_ to predict the degradation of an electrical signal passing along the transatlantic cable.
It is derived as follows:

.. figure:: images/current_flow_leaky_cable.png
   :name: leaky_cable_current_flow
   :alt: Current flow in a leaky cable
   :align: center
   :figwidth: 500

   Current flow in a leaky cable.  **TODO** I don't understand the vertical arrow??


If the voltage is raised at the left hand end of the cable (shown by the deep red in :numref:`leaky_cable_current_flow`), an axial current :math:`i_a` (in :math:`A`) will flow.
This current depends on the voltage gradient according to Ohm's Law, such that:

.. math::

  - \frac{\partial V}{\partial x} = r_a i_a

where :math:`\frac{\partial V}{\partial x}` is the voltage gradient along the cable (in :math:`V/m`), and :math:`r_a` is the resistance (in :math:`\Omega/m`).

If the cable membrane leaks a current of :math:`i_m` (:math:`A/m`) per unit length, then conservation of current gives:

.. math::

  \frac{\partial i_a}{\partial x} = i_m

and, substituting for :math:`i_a` we get:

.. math::

  \frac{\partial}{\partial x}\left( - \frac{1}{r_a}\frac{\partial V}{\partial x} \right) = i_m

There are two sources of membrane leakage current, :math:`i_m`.
First, the capacitance associated with the membrane, :math:`C_m` (approximately 1 :math:`\mu F/cm^2`) results in a current of :math:`C_m\frac{\partial V}{\partial t}`, and secondly the holes or channels in the membrane give :math:`i_{leak}`.
Incorporating these into the equation above gives:

.. math::

   \frac{\partial}{\partial x}\left( - \frac{1}{r_a}\frac{\partial V}{\partial x} \right) = i_m = C_m\frac{\partial V}{\partial t} + i_{leak}

Rearranging gives the **cable equation** (for constant :math:`r_a`):

.. math::

   C_{m}\frac{\partial V}{\partial t} = - \frac{1}{r_a}\frac{\partial^2 V}{\partial x^2} - i_{leak}

where all terms represent *current density* (current per membrane area) and have units of :math:`\mu A/cm^2`.


Theory: Action potentials
-------------------------

.. figure:: images/current_flow_neuron.png
   :name: neuron_current_flow
   :alt: Current flow in a neuron
   :align: center
   :figwidth: 600

   Current flow in a neuron.  **TODO** I don't understand the vertical arrow for V?? direction of voltage? Is one side in/out the cell?

The cable equation above can be used to model the propagation of an action potential along a neuron or any other excitable cell. 
The leak current is associated primarily with the inward movement of sodium ions through the membrane's sodium channel, giving the *inward* membrane current :math:`i_{Na}`, and the outward movement of potassium ions through a membrane potassium channel, giving the *outward* current :math:`i_K` (see :numref:`neuron_current_flow`).
A further small leak current :math:`i_L = g_L\left( V - E_L \right)` associated with chloride and other ions is also included.
**TODO** Need to define E_L and g_L ??
How is i_L related to i_leak above??

.. figure:: images/current_voltage_trajectory.png
   :name: current_voltage_during_action_potential
   :alt: Current-voltage trajectory
   :align: center

   Current-voltage trajectory during an action potential.

When the membrane potential :math:`V` rises due to axial current flow, the sodium (:math:`Na^+`) channels open and the potassium (:math:`K^+`) channels close, such that the membrane potential moves towards the Nernst potential for sodium.
The subsequent decline of the Na channel conductance and the increasing K channel conductance as the voltage drops rapidly repolarises the membrane to its resting potential of -85mV (see :numref:`current_voltage_during_action_potential`).


If we assume that the whole cell is clamped with an axially-uniform potential then we can neglect\ [#]_ the term in the equation above representing the rate of change of axial current along the cable:

.. math::

   - \frac{1}{r_a}\frac{\partial^{2}V}{\partial x^2} \approx 0


and so obtain the membrane potential :math:`V` by integrating the first order ODE:

.. math::

   \frac{dV}{dt} = \frac{- \left( i_{Na} + \ i_K + i_L \right)}{C_m}

.. figure:: images/hodgkin_1952.png
   :name: hodgkin_huxley_schematic
   :alt: CellML schematic cell membrane model
   :align: center
   :figwidth: 500

   A schematic cell diagram describing the current flows across the cell bi-lipid membrane that are captured in the Hodgkin-Huxley model.
   The membrane ion channels are a sodium (Na\ :sup:`+`) channel, a potassium (K\ :sup:`+`) channel, and a leakage (L) channel (for chloride and other ions) through which the currents I\ :sub:`Na`, I\ :sub:`K` and I\ :sub:`L` flow, respectively.

TODO How are the i_L and I_L in the figure related?  Consistent labelling needed?

Interpretation as a CellML model
--------------------------------
We discussed the idea and implementation of *encapsulation* in the :ref:`previous section on the sodium channel<sodium_channel>`, and here it is no different.
We would like to create a model with the encapsulation structure shown in :numref:`cellml_of_hh_model`.

.. figure:: images/hh_encapsulation.png
   :name: cellml_of_hh_model
   :alt: CellML schematic HH model
   :align: center

   The relationship between the ion channels for sodium, potassium and the leakage current, and the membrane and environment components.

As with other aspects of libCellML, there are several options for the model construction process. 
Since we already have potassium and sodium channel models available, it would make sense to be able to reuse these here.
This functionality requires *imports*; their use is demonstrated in `HH Tutorial 2 Creating a model which uses imports`_.

Importing allows all or part of a model to be used in-situ, without needing to manually parse its CellML file and instantiate it as an additional model (as has been the procedure in `HH Tutorial 1 Creating a model using the API`_ and `HH Tutorial 3 Debugging a model`_).  

Simulation and results
----------------------
`HH Tutorial 2 Creating a model which uses imports`_ outlines how smaller models can be used to construct a more complicated model as described above.
In this instance, the models described earlier representing a :ref:`potassium channel<potassium_channel>` and a :ref:`sodium channel<sodium_channel>`, as well as one to represent the leakage concept introduced above can be combined to make the full Hodgkin-Huxley model.


---------------------------

.. rubric:: Footnotes

.. [#] Hodgkin AL and Huxley AF. A quantitative description of membrane current and its application to conduction and excitation in nerve.
       *Journal of Physiology* 117, 500-544, 1952. `PubMed ID: 12991237 <http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?db=pubmed&cmd=Retrieve&dopt=AbstractPlus&list_uids=12991237&query_hl=1&itool=pubmed_docsum>`__

.. [#] http://en.wikipedia.org/wiki/Cable_theory

.. [#] This term is needed when determining the propagation of the action potential, including its wave speed.

.. _HH Tutorial 1 Creating a model using the API: https://libcellml.org/documentation/latest/tutorials/hh_tutorial1/index

.. _HH Tutorial 2 Creating a model which uses imports: https://libcellml.org/documentation/latest/tutorials/hh_tutorial2/index

.. _HH Tutorial 3 Debugging a model: https://libcellml.org/documentation/latest/tutorials/hh_tutorial3/index
