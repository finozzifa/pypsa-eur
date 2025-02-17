..
  SPDX-FileCopyrightText: Contributors to PyPSA-Eur <https://github.com/pypsa/pypsa-eur>

  SPDX-License-Identifier: CC-BY-4.0

.. _wildcards:

#########
Wildcards
#########

It is easy to run PyPSA-Eur for multiple scenarios using the wildcards feature of ``snakemake``.
Wildcards allow to generalise a rule to produce all files that follow a regular expression pattern
which e.g. defines one particular scenario. One can think of a wildcard as a parameter that shows
up in the input/output file names of the ``Snakefile`` and thereby determines which rules to run,
what data to retrieve and what files to produce.

.. note::
    Detailed explanations of how wildcards work in ``snakemake`` can be found in the
    `relevant section of the documentation <https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#wildcards>`__.

.. _cutout_wc:

The ``{cutout}`` wildcard
=========================

The ``{cutout}`` wildcard facilitates running the rule :mod:`build_cutout`
for all cutout configurations specified under ``atlite: cutouts:``.
These cutouts will be stored in a folder specified by ``{cutout}``.

.. _technology:

The ``{technology}`` wildcard
=============================

The ``{technology}`` wildcard specifies for which renewable energy technology to produce availability time
series and potentials using the rule :mod:`build_renewable_profiles`.
It can take the values ``onwind``, ``offwind-ac``, ``offwind-dc``, ``offwind-float``, and ``solar`` but **not** ``hydro``
(since hydroelectric plant profiles are created by a different rule)``

.. _clusters:

The ``{clusters}`` wildcard
===========================

The ``{clusters}`` wildcard specifies the number of buses a detailed
network model should be reduced to in the rule :mod:`cluster_network`.
The number of clusters must be lower than the total number of nodes
and higher than the number of countries. However, a country counts twice if
it has two asynchronous subnetworks (e.g. Denmark or Italy).

.. _opts:

The ``{opts}`` wildcard
=======================

The ``{opts}`` wildcard is used for electricity-only studies. It triggers
optional constraints, which are activated in either :mod:`prepare_network` or
the :mod:`solve_network` step. It may hold multiple triggers separated by ``-``,
i.e. ``Co2L-3h`` contains the ``Co2L`` trigger and the ``3h`` switch. There are
currently:


.. csv-table::
   :header-rows: 1
   :widths: 10,20,10,10
   :file: configtables/opts.csv

.. _sector_opts:

The ``{sector_opts}`` wildcard
==============================

.. warning::
    More comprehensive documentation for this wildcard will be added soon.
    To really understand the options here, look in scripts/prepare_sector_network.py

  # Co2Lx specifies the CO2 target in x% of the 1990 values; default will give default (5%);
  # Co2L0p25 will give 25% CO2 emissions; Co2Lm0p05 will give 5% negative emissions
  # xH is the temporal resolution; 3h is 3-hourly, i.e. one snapshot every 3 hours
  # single letters are sectors: T for land transport, H for building heating,
  # B for biomass supply, I for industry, shipping and aviation,
  # A for agriculture, forestry and fishing
  # solar+c0.5 reduces the capital cost of solar to 50\% of reference value
  # solar+p3 multiplies the available installable potential by factor 3
  # seq400 sets the potential of CO2 sequestration to 400 Mt CO2 per year
  # dist{n} includes distribution grids with investment cost of n times cost in data/costs.csv
  # for myopic/perfect foresight cb states the carbon budget in GtCO2 (cumulative
  # emissions throughout the transition path in the timeframe determined by the
  # planning_horizons), be:beta decay; ex:exponential decay
  # cb40ex0 distributes a carbon budget of 40 GtCO2 following an exponential
  # decay with initial growth rate 0

The ``{sector_opts}`` wildcard is only used for sector-coupling studies.

.. csv-table::
   :header-rows: 1
   :widths: 10,20,10,10
   :file: configtables/sector-opts.csv

.. _planning_horizons:

The ``{planning_horizons}`` wildcard
====================================

.. warning::
    More comprehensive documentation for this wildcard will be added soon.

The ``{planning_horizons}`` wildcard is only used for sector-coupling studies.
It takes years as values, e.g. 2020, 2030, 2040, 2050.
