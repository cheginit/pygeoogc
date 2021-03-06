.. image:: https://raw.githubusercontent.com/cheginit/HyRiver-examples/main/notebooks/_static/pygeoogc_logo.png
    :target: https://github.com/cheginit/HyRiver

|

.. image:: https://joss.theoj.org/papers/b0df2f6192f0a18b9e622a3edff52e77/status.svg
    :target: https://joss.theoj.org/papers/b0df2f6192f0a18b9e622a3edff52e77
    :alt: JOSS

|

.. |pygeohydro| image:: https://github.com/cheginit/pygeohydro/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/pygeohydro/actions/workflows/test.yml
    :alt: Github Actions

.. |pygeoogc| image:: https://github.com/cheginit/pygeoogc/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/pygeoogc/actions/workflows/test.yml
    :alt: Github Actions

.. |pygeoutils| image:: https://github.com/cheginit/pygeoutils/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/pygeoutils/actions/workflows/test.yml
    :alt: Github Actions

.. |pynhd| image:: https://github.com/cheginit/pynhd/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/pynhd/actions/workflows/test.yml
    :alt: Github Actions

.. |py3dep| image:: https://github.com/cheginit/py3dep/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/py3dep/actions/workflows/test.yml
    :alt: Github Actions

.. |pydaymet| image:: https://github.com/cheginit/pydaymet/actions/workflows/test.yml/badge.svg
    :target: https://github.com/cheginit/pydaymet/actions/workflows/test.yml
    :alt: Github Actions

=========== ==================================================================== ============
Package     Description                                                          Status
=========== ==================================================================== ============
PyNHD_      Navigate and subset NHDPlus (MR and HR) using web services           |pynhd|
Py3DEP_     Access topographic data through National Map's 3DEP web service      |py3dep|
PyGeoHydro_ Access NWIS, NID, HCDN 2009, NLCD, and SSEBop databases              |pygeohydro|
PyDaymet_   Access Daymet for daily climate data both single pixel and gridded   |pydaymet|
PyGeoOGC_   Send queries to any ArcGIS RESTful-, WMS-, and WFS-based services    |pygeoogc|
PyGeoUtils_ Convert responses from PyGeoOGC's supported web services to datasets |pygeoutils|
=========== ==================================================================== ============

.. _PyGeoHydro: https://github.com/cheginit/pygeohydro
.. _PyGeoOGC: https://github.com/cheginit/pygeoogc
.. _PyGeoUtils: https://github.com/cheginit/pygeoutils
.. _PyNHD: https://github.com/cheginit/pynhd
.. _Py3DEP: https://github.com/cheginit/py3dep
.. _PyDaymet: https://github.com/cheginit/pydaymet

PyGeoOGC: Retrieve Data from RESTful, WMS, and WFS Services
-----------------------------------------------------------

.. image:: https://img.shields.io/pypi/v/pygeoogc.svg
    :target: https://pypi.python.org/pypi/pygeoogc
    :alt: PyPi

.. image:: https://img.shields.io/conda/vn/conda-forge/pygeoogc.svg
    :target: https://anaconda.org/conda-forge/pygeoogc
    :alt: Conda Version

.. image:: https://codecov.io/gh/cheginit/pygeoogc/branch/main/graph/badge.svg
    :target: https://codecov.io/gh/cheginit/pygeoogc
    :alt: CodeCov

.. image:: https://img.shields.io/pypi/pyversions/pygeoogc.svg
    :target: https://pypi.python.org/pypi/pygeoogc
    :alt: Python Versions

.. image:: https://pepy.tech/badge/pygeoogc
    :target: https://pepy.tech/project/pygeoogc
    :alt: Downloads

|

.. image:: https://img.shields.io/badge/security-bandit-green.svg
    :target: https://github.com/PyCQA/bandit
    :alt: Security Status

.. image:: https://www.codefactor.io/repository/github/cheginit/pygeoogc/badge
   :target: https://www.codefactor.io/repository/github/cheginit/pygeoogc
   :alt: CodeFactor

.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
    :target: https://github.com/psf/black
    :alt: black

.. image:: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white
    :target: https://github.com/pre-commit/pre-commit
    :alt: pre-commit

.. image:: https://mybinder.org/badge_logo.svg
    :target: https://mybinder.org/v2/gh/cheginit/HyRiver-examples/main?urlpath=lab/tree/notebooks
    :alt: Binder

|

Features
--------

PyGeoOGC is a part of `HyRiver <https://github.com/cheginit/HyRiver>`__ software stack that
is designed to aid in watershed analysis through web services. This package provides
general interfaces to web services that are based on
`ArcGIS RESTful <https://en.wikipedia.org/wiki/Representational_state_transfer>`__,
`WMS <https://en.wikipedia.org/wiki/Web_Map_Service>`__, and
`WFS <https://en.wikipedia.org/wiki/Web_Feature_Service>`__. Although
all these web service have limits on the number of features per requests (e.g., 1000
objectIDs for a RESTful request or 8 million pixels for a WMS request), PyGeoOGC divides
the requests into smaller chunks, under-the-hood, and then merges the results. Moreover,
under-the-hood, this package uses ``requests-cache`` for persistent caching that can improve
the performance significantly.

There is also an inventory of URLs for some of these web services in form of a class called
``ServiceURL``. These URLs are in four categories: ``ServiceURL().restful``,
``ServiceURL().wms``, ``ServiceURL().wfs``, and ``ServiceURL().http``. These URLs provide you
with some examples of the services that PyGeoOGC supports. All the URLs are read from a YAML
file located `here <pygeoogc/static/urls.yml>`_. If you have success using PyGeoOGC with a web
service please consider submitting a request to be added to this URL inventory, located at
``pygeoogc/static/urls.yml``.

PyGeoOGC has three main classes:

* ``ArcGISRESTful``: This class can be instantiated by providing the target layer URL.
  For example, for getting Watershed Boundary Data we can use ``ServiceURL().restful.wbd``.
  By looking at the web service's
  `website <https://hydro.nationalmap.gov/arcgis/rest/services/wbd/MapServer>`_
  we see that there are nine layers. For example, 1 for 2-digit HU (Region), 6 for 12-digit HU
  (Subregion), and so on. We can pass the URL to the target layer directly, like this
  ``f"{ServiceURL().restful.wbd}/6"`` or as a separate argument via ``layer``.

  Afterward, we request for the data in two steps. First, we need to get
  the target object IDs using ``oids_bygeom`` (within a geometry), ``oids_byfield`` (specific
  field IDs), or ``oids_bysql`` (any valid SQL 92 WHERE clause) class methods. Then, we can get
  the target features using ``get_features`` class method. The returned response can be converted
  into a GeoDataFrame using ``json2geodf`` function from
  `PyGeoUtils <https://github.com/cheginit/pygeoutils>`__.

* ``WMS``: Instantiation of this class requires at least 3 arguments: service URL, layer
  name(s), and output format. Additionally, target CRS and the web service version can be provided.
  Upon instantiation, we can use ``getmap_bybox`` method class to get the target raster data
  within a bounding box. The box can be in any valid CRS and if it is different from the default
  CRS, EPSG:4326, it should be passed using ``box_crs`` argument. The service response can be
  converted into a ``xarray.Dataset`` using ``gtiff2xarray`` function from PyGeoUtils.

* ``WFS``: Instantiation of this class is similar to ``WMS``. The only difference is that
  only one layer name can be passed. Upon instantiation there are three ways to get the data:

  - ``getfeature_bybox``: Get all the target features within a bounding box in any valid CRS.
  - ``getfeature_byid``: Get all the target features based on the IDs. Note that two arguments
    should be provided: ``featurename``, and ``featureids``. You can get a list of valid feature
    names using ``get_validnames`` class method.
  - ``getfeature_byfilter``: Get the data based on any valid
    `CQL <https://docs.geoserver.org/latest/en/user/tutorials/cql/cql_tutorial.html>`__ filter.

  You can convert the returned response of this function to a GeoDataFrame using ``json2geodf``
  function from PyGeoUtils package.

You can find some example notebooks `here <https://github.com/cheginit/HyRiver-examples>`__.

You can even try using PyGeoOGC without installing it on you system by clicking on the binder
badge below the PyGeoOGC banner. A Jupyter notebook instance with the software stack
pre-installed will be launched in your web browser and you can start coding!

Please note that since this project is in early development stages, while the provided
functionalities should be stable, changes in APIs are possible in new releases. But we
appreciate it if you give this project a try and provide feedback. Contributions are most welcome.

Moreover, requests for additional functionalities can be submitted via
`issue tracker <https://github.com/cheginit/pygeoogc/issues>`__.

Installation
------------

You can install PyGeoOGC using ``pip``:

.. code-block:: console

    $ pip install pygeoogc

Alternatively, PyGeoOGC can be installed from the ``conda-forge`` repository
using `Conda <https://docs.conda.io/en/latest/>`__:

.. code-block:: console

    $ conda install -c conda-forge pygeoogc

Quick start
-----------

We can access
`NHDPlus HR <https://edits.nationalmap.gov/arcgis/rest/services/NHDPlus_HR/NHDPlus_HR/MapServer>`__
via RESTful service,
`National Wetlands Inventory <https://www.fws.gov/wetlands/>`__ from WMS, and
`FEMA National Flood Hazard <https://www.fema.gov/national-flood-hazard-layer-nfhl>`__
via WFS. The output for these functions are of type ``requests.Response`` that
can be converted to ``GeoDataFrame`` or ``xarray.Dataset`` using
`PyGeoUtils <https://github.com/cheginit/pygeoutils>`__.

Let's start the National Map's NHDPlus HR web service. We can query the flowlines that are
within a geometry as follows:

.. code-block:: python

    from pygeoogc import ArcGISRESTful, WFS, WMS, ServiceURL
    import pygeoutils as geoutils
    from pynhd import NLDI

    basin_geom = NLDI().get_basins("01031500").geometry[0]

    hr = ArcGISRESTful(ServiceURL().restful.nhdplushr, 2, outformat="json")

    hr.oids_bygeom(basin_geom, "epsg:4326")
    resp = hr.get_features()
    flowlines = geoutils.json2geodf(resp)

Note ``oids_bygeom`` has three additional arguments: ``sql_clause``, ``spatial_relation``,
and ``distance``. We can use ``sql_clause`` for passing any valid SQL WHERE clause and
``spatial_relation`` for specifying the target predicate such as
intersect, contain, cross, etc.. The default predicate is intersect
(``esriSpatialRelIntersects``). We can use ``distance`` for specifying the buffer
distance from the input geometry for getting features.

We can also submit a query based on IDs of any valid field in the database. If the measure
property is desired you can pass ``return_m`` as ``True`` to the ``get_features`` class method:

.. code-block:: python

    hr.oids_byfield("PERMANENT_IDENTIFIER", ["103455178", "103454362", "103453218"])
    resp = hr.get_features(return_m=True)
    flowlines = geoutils.json2geodf(resp)

Additionally, any valid SQL 92 WHERE clause can be used. For more details look
`here <https://developers.arcgis.com/rest/services-reference/query-feature-service-.htm#ESRI_SECTION2_07DD2C5127674F6A814CE6C07D39AD46>`__.
For example, let's limit our first request to only include catchments with
areas larger than 0.5 sqkm.

.. code-block:: python

    hr.oids_bygeom(basin_geom, geo_crs="epsg:4326", sql_clause="AREASQKM > 0.5")
    resp = hr.get_features()
    catchments = geoutils.json2geodf(resp)

A WMS-based example is shown below:

.. code-block:: python

    wms = WMS(
        ServiceURL().wms.fws,
        layers="0",
        outformat="image/tiff",
        crs="epsg:3857",
    )
    r_dict = wms.getmap_bybox(
        basin_geom.bounds,
        1e3,
        box_crs="epsg:4326",
    )
    wetlands = geoutils.gtiff2xarray(r_dict, basin_geom, "epsg:4326")

Query from a WFS-based web service can be done either within a bounding box or using
any valid `CQL filter <https://docs.geoserver.org/stable/en/user/tutorials/cql/cql_tutorial.html>`__.

.. code-block:: python

    wfs = WFS(
        ServiceURL().wfs.fema,
        layer="public_NFHL:Base_Flood_Elevations",
        outformat="esrigeojson",
        crs="epsg:4269",
    )
    r = wfs.getfeature_bybox(basin_geom.bounds, box_crs="epsg:4326")
    flood = geoutils.json2geodf(r.json(), "epsg:4269", "epsg:4326")

    layer = "wmadata:huc08"
    wfs = WFS(
        ServiceURL().wfs.waterdata,
        layer=layer,
        outformat="application/json",
        version="2.0.0",
        crs="epsg:4269",
    )
    r = wfs.getfeature_byfilter(f"huc8 LIKE '13030%'")
    huc8 = geoutils.json2geodf(r.json(), "epsg:4269", "epsg:4326")

.. image:: https://raw.githubusercontent.com/cheginit/HyRiver-examples/main/notebooks/_static/sql_clause.png
    :target: https://github.com/cheginit/HyRiver-examples/blob/main/notebooks/webservices.ipynb


Contributing
------------

Contributions are appreciated and very welcomed. Please read
`CONTRIBUTING.rst <https://github.com/cheginit/pygeoogc/blob/main/CONTRIBUTING.rst>`__
for instructions.
