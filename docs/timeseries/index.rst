.. currentmodule:: gwpy.timeseries.core

#######################
The :class:`TimeSeries`
#######################

.. code-block:: python

   >>> from gwpy.timeseries import TimeSeries


Gravitational-wave detectors are time-domain instruments, attempting to record gravitational wave amplitude as a differential change in the lengths of each of the interferometer arms.
Alongside these data, thousands of auxiliary instrumental control and error signals and environmental monitors are recorded in real-time and recorded to disk as GWF-format 'frame' files.
To learn more about this particular data format, take a look at the specification document `LIGO-T970130 <https://dcc.ligo.org/LIGO-T970130/public>`_.

GWpy represents these data through the :class:`TimeSeries` object, a :class:`numpy.ndarray` containing the data themselves and a full set of metadata.

Any `TimeSeries` can be generated from a standard `~numpy.ndarray` or `list` by providing the data and the minimal :attr:`~TimeSeries.epoch` and :attr:`~TimeSeries.sample_rate` metadata::

   >>> series = TimeSeries([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], epoch=1000000000, sample_rate=1)
   >>> print(series)
   TimeSeries([ 1  2  3  4  5  6  7  8  9 10],
              name: None,
              unit: None,
              epoch: 2011-09-14 01:46:59.000,
              channel: None,
              sample_rate: 1 Hz)

The full set of metadata that can be provided is as follows:

.. autosummary::

   ~TimeSeries.name
   ~TimeSeries.unit
   ~TimeSeries.epoch
   ~TimeSeries.sample_rate
   ~TimeSeries.channel

=============================
Accessing interferometer data
=============================

As described above, the data from each instrument are archived in gravitational-wave frame files.
These files are stored on disk by the LIGO Data Grid and can be either accessed either directly or remotely.

The `TimeSeries` for a given :class:`~gwpy.detector.channel.Channel` can be read from disk using the :meth:`TimeSeries.read` `classmethod` as follows::

    >>> from gwpy.timeseries import TimeSeries
    >>> data = TimeSeries.read('/archive/frames/A6/L0/LLO/L-R-10670/L-R-1067042880-32.gwf', 'L1:PSL-ODC_CHANNEL_OUT_DQ')

Alternatively, the data can be downloaded on-the-fly `Network Data Server <https://www.lsc-group.phys.uwm.edu/daswg/projects/nds-client.html>`_ via the :meth:`TimeSeries.fetch` `classmethod`::

    >>> from gwpy.timeseries import TimeSeries
    >>> data = TimeSeries.fetch('L1:PSL-ODC_CHANNEL_OUT_DQ', 1067042880, 1067042912)

Both of the above snippets will return identical `TimeSeries`.
For more details on accessing data via either of these sources, please read:

.. toctree::
   :maxdepth: 1

   gwf
   nds

========================
`TimeSeries` collections
========================

A single `TimeSeries` is meant to represent one stream of contiguous data from a single source.
There are two collections provided that allow you to bundle multiple `TimeSeries` from a single source, and multiple `TimeSeries` of the same epoch from a number of sources:

--------------------
The `TimeSeriesList`
--------------------

The `TimeSeriesList` is an extension of the builtin `list` type to allow easy collection and manipulation of a number of `TimeSeries` from a single source, say different epochs from a single :class:`~gwpy.detector.channel.Channel`.
This object comes with a few handy methods that make combining multiple epochs very simple:

.. autosummary::

   ~TimeSeriesList.coalesce
   ~TimeSeriesList.join

--------------------
The `TimeSeriesDict`
--------------------

The `TimeSeriesDict` does for multiple channels what the `TimeSeriesList` does for multiple epochs, allowing easy collection of `TimeSeries` from a single epochs but multiple sources.
The most immediate itilities of this object are the :meth:`TimeSeriesDict.read` and :meth:`TimeSeriesDict.fetch` methods, allowing users to read and download respectively data for numerous channels in a single command.
See the full reference for what other functionality is available.

.. _plotting-a-timeseries:

=======================
Plotting a `TimeSeries`
=======================

The `TimeSeries` object comes with its own :meth:`~TimeSeries.plot` method, which will quickly construct a :class:`~gwpy.plotter.timeseries.TimeSeriesPlot`.
In this example, which we will use in various places in these pages, we download ten minutes of gravitational-wave strain readout data from the LIGO Hanford Observatory, and display it:

.. plot:: timeseries/timeseries_plot.py
   :include-source:

=========================
`TimeSeries` applications
=========================

.. toctree::
   :maxdepth: 1

   filtering
   spectra
   statevector

======================
`TimeSeries` reference
======================

.. autoclass:: TimeSeries
   :members:
