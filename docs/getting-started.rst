Getting Started
===============

.. toctree::
   :maxdepth: 2
   :caption: Contents:


Installation
------------

AVWX is available on PyPI. Note: the package name is ``avwx-engine``, but the import is ``avwx``

.. code-block:: bash

  pip install avwx-engine

Tutorial
--------

Let's run through a quick example of fetching and parsing a METAR and TAF.

.. code-block:: python

  >>> import avwx
  >>> jfk_metar = avwx.Metar('KJFK')
  >>> jfk_metar.update()
  True
  >>> jfk_metar.raw
  'KJFK 281651Z 33021G25KT 10SM FEW060 M08/M23 A3054 RMK AO2 SLP339 T10831228'
  >>> jfk_metar.data['Flight-Rules']
  'VFR'
  >>> jfk_metar.summary
  'Winds NNW-330 at 21kt gusting to 25kt, Vis 10sm, Temp -08C, Dew -23C, Alt 30.54inHg, Few clouds at 6000ft'
  >>> jfk_metar.station_info['Name']
  'John F Kennedy International Airport'

Here, we create a METAR object and initialize it to pull data for JFK International airport. The update call fetches the current report, parses it into its individual components, and formats the translations. We then view the original report, the calculated flight rules, and a summary string from the translations. We can also see details of the station if available.

.. code-block:: python

  >>> hnl_taf = avwx.Taf('PHNL')
  >>> hnl_taf.update()
  True
  >>> hnl_taf.raw
  'PHNL 312058Z 3121/0124 07012G19KT P6SM FEW030 SCT050 FM010500 06007KT P6SM FEW025 SCT045 FM012000 07012G19KT P6SM OVC030 SCT050'
  >>> len(hnl_taf.data['Forecast'])
  3
  >>> for line in hnl_taf.data['Forecast']:
  ...   start, end, rules = line['Start-Time'], line['End-Time'], line['Flight-Rules']
  ...   print(f'{rules} from {start[:2]}-{start[2:]}:00 to {end[:2]}-{end[2:]}:00')
  ...
  VFR from 31-21:00 to 01-05:00
  VFR from 01-05:00 to 01-20:00
  MVFR from 01-20:00 to 01-24:00

Here we start of the same with the Taf object, this time for Honolulu. Because TAFs are forecasts, they contain multiple time periods. Here, we have three: a base and two amendments. Our code shows the different forecasted flight rules for each time period (day-hour). Taf objects have most of the same attributes as Metar objects, so we could also grab the station info if we needed to.