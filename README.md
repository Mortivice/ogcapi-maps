# OGC API - Maps

This GitHub repository contains [OGC](https://www.ogc.org/)'s multi-part standard for querying and retrieving maps on the web, "OGC API - Maps". The draft specification is available in [HTML](http://docs.ogc.org/DRAFTS/20-058.html) and [PDF](http://docs.ogc.org/DRAFTS/20-058.pdf).

A [Map](https://en.wikipedia.org/wiki/Map) provides a visual representation of relationships between things within a defined space. OGC API - Maps defines a standardized way to request a map to serve multiple purposes (e.g. displaying maps in web pages, mapping software, etc.). OGC API - Maps defines number of request parameters to control e.g., the dimensions, background, portions of the dataset to be included in the map, coordinate reference system used.

OGC API - Maps is part of the suite of [OGC API standards](https://ogcapi.ogc.org/) that define modular API building blocks to enable access and use of location (i.e. geospatial) information in a consistent way. Information about other OGC APIs can be found [online](https://ogcapi.ogc.org/).

## Overview

*IMPORTANT NOTE: The description in this README.md can be older than the one in the [standard](core/standard) draft. In case of discrepancy, the standard draft takes precedence (last update 2022-11-17).*

OGC API - Maps specifies building blocks for Web APIs providing maps representing geospatial data. These building blocks can be combined with several origins such as the ones suggested in the Origin Conformance Classes. To build a full URL substitute the "..." for the origin URL (e.g., `.../map` becomes `{datasetRoot}/collections/{collectionId}/map`).

**Get a map**

```
GET .../map
```
Requests the default representation of the map. Additional parameters can be specified to offer more control on the map being returned.


**Specify dimensions**

```
GET .../map?width=600&height=400
```
Width and height allow you to define the resolution of the returned map.

**Spatial subsetting**

```
GET .../map?bbox-crs=[OGC:CRS84]&bbox=160.6,-55.95,-170,-25.89
```
Geospatial datasets can cover large areas. The Bounding Box (bbox) option allows you to specify a portion of the data for which you wish to retrieve a map. `bbox` is defined using the `bbox-crs` specified in the request (defaulting to CRS84 if not specified).

```
GET .../map?subset-crs=[OGC:CRS84]&subset=Lat(-55.95:-25.89),Lon(160.6:-170)
```
As an alternative to `bbox`, the `subset` query parameter can be used, with a syntax explicitly stating the axis for each bounding coordinate pairs.

**Temporal subsetting**

```
GET .../map?datetime=2020-08-20T14:12:05Z
```
Maps can be obtained for specific date and time.

```
GET .../map?subset=datetime("2020-08-20T14:12:05Z")
```
Date and time can also be specified using the subset query parameter.

**Styled maps**

```
GET .../styles/{styleId}/map
```
(In combination with _OGC API - Styles_) Requests the map to be returned in a specific style. This allows you to define the appearance of the map to suit your needs.

**Alternate map CRS**

```
GET .../map?crs=[EPSG:3395]
```
Maps use [Coordinate Reference Systems](https://en.wikipedia.org/wiki/Spatial_reference_system) (CRS) to define their position within a space (e.g., the Earth's surface). OGC API - Maps allows maps to be requested in any CRS supported by the server, and defaults to the native/storage CRS (not necessarily CRS84). This example will return the map according to a [World Mercator Projection](https://en.wikipedia.org/wiki/Mercator_projection) CRS.

**Putting it all together**

Multiple parameters can be combined together for a single request, e.g.:

```
GET .../styles/{styleId}/map?
   width=600&height=400&
   bbox-crs=[OGC:CRS84]&bbox=160.6,-55.95,-170,-25.89&
   datetime=2020-08-20T14:12:05Z&
   crs=[EPSG:3395]
   
GET .../styles/{styleId}/map?
   crs=[EPSG:3395]&
   width=600&height=400&
   subset-crs=[OGC:CRS84]&
   subset=Lat(-55.95:-25.89),Lon(160.6:-170),datetime("2020-08-20T14:12:05Z")
```

## Conformance Classes

### Resources Conformance Classes

1. [Map "Core"](http://docs.ogc.org/DRAFTS/20-058.html#rc_maps_core) specifies a `.../map` sub-resource allowing to retrieve a map from some origin providing geospatial data (see _Origin Conformance Classes_).

2. [Map "TileSets"](http://docs.ogc.org/DRAFTS/20-058.html#rc_maps_tileSets-list) specifies how to retrieve map tilesets (e.g., `.../map/tiles/{tileMatrixSetId}`) metadata and map tiles (e.g., `.../map/tiles/{tileMatrixSetId}/{tileMatrix}/{tileRow}/{tileCol}`) based on _OGC API - Tiles_ building blocks.

### Parameters Conformance Classes

3. [Map "Scaling"](http://docs.ogc.org/DRAFTS/20-058.html#rc_scaling) (`width` and `height`) allows to specify a map resolution
4. [Map "Spatial subsetting"](http://docs.ogc.org/DRAFTS/20-058.html#rs_spatial-subsetting) (`bbox`, `bbox-crs`-, `subset` with `X`,`Y`,`E`,`N`,`Lat`,`Lon` axes) and `subset-crs`) allows to retrieve a portion of a map
5. [Map "Background"](http://docs.ogc.org/DRAFTS/20-058.html#rc_background) (`bgcolor` and `transparent`) allows to toggle background transparency and select a background color
6. [Map "Collections Selection"](http://docs.ogc.org/DRAFTS/20-058.html#rc_collections-selection) (`collections=`) allows to select geospatial data resources to include in the map
7. [Map "Date & Time"](http://docs.ogc.org/DRAFTS/20-058.html#rc_datetime) (`datetime`, `subset` with `datetime` axis) allows to retrieve a map for a specific date and time
8. [Map "CRS"](http://docs.ogc.org/DRAFTS/20-058.html#rc_crs) (`crs`) allows to select an output CRS other than the default native/storage CRS
9. [Map "Cartographic Layout"](http://docs.ogc.org/DRAFTS/20-058.html#rc_cartographic-layout) allows enable and customize cartographic layout elements such as the legend, scale, compass

### Origin Conformance Classes

10. [Dataset Maps](http://docs.ogc.org/DRAFTS/20-058.html#rc_datasetMaps) (`{datasetRoot}/map`) allows to retrieve a map for an OGC API dataset as a whole, in combination with OGC API Common - Part 1: Core
11. [Collection Maps](http://docs.ogc.org/DRAFTS/20-058.html#rc_geoDataResourceMaps) (`{datasetRoot}/collections/{collectionId}/map`) allows to retrieve a map for a specific OGC API collection, in combination with OGC API Common - Part 2: Geospatial Data.
12. ["Styled Map"](http://docs.ogc.org/DRAFTS/20-058.html#rc_styledMaps) (`({datasetRoot}|{datasetRoot}/collections/{collectionId})/styles/{styleId}/map`) allows to retrieve a map for a particular style, in combination with OGC API - Styles

### Representations Conformance Classes

13. ["OpenAPI definition"](http://docs.ogc.org/DRAFTS/20-058.html#rc_oas30_definition) specifies additional requirements pertaining to operationIDs to identify Maps building blocks resources
14. ["PNG"](http://docs.ogc.org/DRAFTS/20-058.html#rc_png) specifies the ability to return maps in the PNG format
15. ["JPEG"](http://docs.ogc.org/DRAFTS/20-058.html#rc_jpeg) specifies the ability to return maps in the JPEG format
16. ["TIFF"](http://docs.ogc.org/DRAFTS/20-058.html#rc_tiff) specifies the ability to return maps in the TIFF format (ideally, GeoTIFF)
17. ["HTML"](http://docs.ogc.org/DRAFTS/20-058.html#rc_html) specifies the ability to return maps as an HTML document (which might provide an interactive map)

## OpenAPI Definition

OpenAPI building blocks, as well as a complete example OpenAPI definition of a Web API implementing this standard is [available here](https://github.com/opengeospatial/ogcapi-maps/tree/master/openapi). The README in that directory contains more information on how these building blocks can be assembled, and how an API description document can be bundled using `swagger-cli`.

The API definition can also be visualized [here with SwaggerUI](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/opengeospatial/ogcapi-maps/master/openapi/ogcapi-maps-1.bundled.json), including a working implementation which can be experimented with to try out the different end-points and responses.

## Implementations

Several implementations of the draft standard exist:

[Implementations of the draft specification / demo services](./implementations.adoc)

## Relation to other OGC standards and previous works

OGC API - Maps follows on the footsteps of the [OGC](http://opengeospatial.org)'s Web Map Service (WMS) by enabling client applications to request maps of geospatial information on the web. However, OGC API - Maps is completely different from WMS, as OGC API - Maps focuses on a simple RESTful core specified as reusable [OpenAPI](http://openapis.org) components.

This is the [CURRENT](http://docs.opengeospatial.org/DRAFTS/20-058.html) working version of this initiative. An [Old](http://docs.opengeospatial.org/per/19-069.html) version of the specification was a deliverable in [Testbed-15](https://www.ogc.org/projects/initiatives/testbed15).

The OGC API - Maps, [OGC API - Styles](https://github.com/opengeospatial/ogcapi-styles), and [OGC API - Tiles](https://github.com/opengeospatial/ogcapi-tiles) specifications are closely related and should be considered complementary.

### Extensions

Extensions may be defined in the future to provide additional capabilities:

- [#97 **Cartographic Layout**](https://github.com/opengeospatial/ogcapi-maps/issues/97) Due to the complexity and flexibility of defining cartographic layout elements, it might be justified to move those capabilities in a separate extension Part 2 document in order to complete it without delaying _Part 1: Core_.
- [#42 **Custom Styles**](https://github.com/opengeospatial/ogcapi-maps/issues/42) Defining how a client can provide a style with which to render a map (other than by publishing a style through _OGC API - Styles_)
- [#81 **GetFeatureInfo**](https://github.com/opengeospatial/ogcapi-maps/issues/81) _OGC API - Maps - Part 1: Core_ does not include an equivalent of WMS _GetFeatureInfo_, however the specification specifies how a majority of the use cases for it can be handled by also implementing API building blocks from _OGC API - Features_ or _OGC API - Coverages_ alongside the Maps implementation. The specification also describes how dynamic maps might be better handled by using OGC API - Tiles and vector/coverage tiles for use cases that previously used _GetFeatureInfo_ in WMS. However, an extension for _GetFeatureInfo_ might still be standardized if the need for it is still there.

## Communication

Join the [WMS mailing list](https://lists.ogc.org/mailman/private/ogcapi-maps.swg/).

Most work on the specification takes place in [GitHub issues](https://github.com/opengeospatial/ogcapi-maps/issues),
so browse there to get a good idea of what is happening, as well as past decisions.

## Contributing

The contributor understands that any contributions, if accepted by the OGC Membership (and eventually the ISO/TC 211), shall be incorporated into the relevant OGC standards documents and that all copyright and intellectual property shall be vested to the OGC.

The WMS Standards Working Group (SWG) is the group at OGC that is responsible for the stewardship of the standard being developed in this GitHub repository, but is working to do as much work in public as possible.

* [Open issues](https://github.com/opengeospatial/ogcapi-maps/issues)
* [Copy of License Language](https://raw.githubusercontent.com/opengeospatial/ogcapi-maps/master/LICENSE)

Pull Requests from contributors are welcomed. However, please note that by sending a Pull Request or Commit to this GitHub repository, you are agreeing to the terms in the [Observer Agreement](https://portal.ogc.org/files/?artifact_id=92169).
