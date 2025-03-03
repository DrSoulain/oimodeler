Overview
========

oimodeler is a project aiming at developping a modular and easily expandable python-based modelling software for optical interferometry. The project started end of 2021, and the software, although fully functional, is currently in an early stage of development.


Software modularity
-------------------

As shown in the diagram below, the oimodeler software is composed of modules. Models can be created with the **oimModel** module and various **oimComponents** which contains models parameters as **oimParam**. Interferometric data can be loaded into a **oimData** object from standard oifits files. This module also allows to load flux/spectra in various format. **oimData** can optionallly be filtered using the **oimDataFilter** class with various **oimFilterComponents**. **oimData** and **oimModel** can be plugged into a **oimSimulator** to simulate data from the model at the same spatial/spectral coordinates than the data. The module also allow to compute the model/data chi2. The  **oimData** and **oimModel** can also be plugged into a **oimFitter** to perform model fitting. Currently only a simple emcee fitter is implemented. **oimPlots** contains plotting functions for oifits data and oimodeler objects. **oimUtils** contains various functions to manipulate oifits data.

.. image:: _static/diagram.png
  :alt: Alternative text


Modules 
-------


oimModel
^^^^^^^^

The oimModel module is dedicated to the creation of models for optical interferometry. Models are modular and composed of one or many oimComponents. they can produce complex coherent flux and images and can be lpug into oimSimulator and/or oimFitter for data analysis and modelling.

oimComponent
^^^^^^^^^^^^^

The oimComponents module deals with model components that can be defined analytically in the Fourier plan (e.g. Uniform Disks, 2D-Gaussian distribution...) or image plan (useful if no analytical formula in the Fourier plan is available). oimComponent can also be used to wrap external code (functions computing images, radial profiles, and hyperspectral cube), or image-fit files (for instance produced using a radiative transfer model). oimComponent can be easily subclassed to create new customs components.

oimParam
^^^^^^^^

The oimParam module contains basic model parameters (oimParam used for the definition of oimComponent parameter), as well as parameter linkers (oimParamLinker), normalizer (oimParamNormalize) and advanced parameter interpolators (oimParamInterpolator) that allow to build chromatic and time dependent models.

oimData
^^^^^^^
The oimData module allows to encapsulate interferometric (also photometric and spectroscopic) data. The oimData allow to retrieve the original data as a list of astropy.io.fits.hdulist (oimData.data) but also provide optimization of the data as vector/structure for faster model fitting. OimDataFilter can be plugged to the oimData to filter the data.

.. warning::
    photometric and spectroscopic data not yet implemented

oimDataFilter
^^^^^^^^^^^^^

oimDataFilter is dedicated to filtering and modifying data. It allows data selection (truncating, removing arrays, and flagging) based on various criteria (wavelengths, SNR ...), and other data manipulation such as smoothing and binning of the data.

oimSimulator
^^^^^^^^^^^^

The oimSimulator module is the basic module for model (oimModel object) and data (oimData object) comparison. It allows to simulate a new dataset (stored in oimSimulator.simulatedData) with the same quantities and spatial/spectral coordinated of an oimData and using the oimModel. It also allows to compute chi2 for data and model comparison. The oimSimulator is fully compatible with the OIFITS2 format and can simulated any kind of data type from an oifits file (classic VIS2DATA, VISAMP in absolue, differential and correlated flux...)

oimFitter
^^^^^^^^^

The oimFitter module is dedicated to model fitting. The parent class oimFitter is an abstract class that can be subclassed to implement various fitter. Currently only a simple emcee-based fitter is implemented oimEmceeFitter.

oimPlots
^^^^^^^^

oimPlots contains various plotting tools for oifits data and oimodeler objects. The oimAxes is a subclass of matplotlib.pyplot.Axes class with methods dedicated to produce plot data oifits data in the astropy.io.fits.hdulist format.

oimUtils
^^^^^^^^

oimUtils contains various functions to manipulate oifits data such as : retrieving baselines names, length, orientation, getting spatial frequencies,  creating new oifits arrays in astropy.io.fits.hudlist format...

Expandability
-------------

oimodeler is written to allow easy implementation of new model component, fitters, data filters, parameter intepolators, data loader (e.g. for non-oifits format data), and plots. Feel free to contact `Anthony Meilland <mailto://ame@oca.eu>`_ if you developped custom features and want them to be included in the oimodeler distribution and become a oimodeler contributer.

