## Mémo de conversion d'un ECW en geotiff
###### Dernière modif tuto : 30/08/2016

### Liens utiles
* Source : [Vive les SIG libres !](http://vivelessig.blogspot.fr/2015/08/se-liberer-du-format-proprietaire-ecw.html) : Se libérer du format propriétaire ECW et passer au Geotiff en gardant un super rapport qualité/poids
* [Forum SIG](http://www.forumsig.org/showthread.php/40755-Support-de-l-ECW) : QGIS Server / support de l'ECW
* [Doc GDAL](http://www.gdal.org/) : Documentation officielle à aller consulter pour plus de configurations !
<br>


### Pré-requis
* Téléchargement du setup de l'OSGeo4W (en 64bit si possible) sur leur site : https://trac.osgeo.org/osgeo4w/
* Installation des "Commandline_Utilities" suivantes : gdal ; gdal-dev ; libgeotiff
* Installation des "Libs" suivantes : gdal ; gdal-dev ; gdal-dev-ecw ; gdal-ecw ; libgeotiff ; libjpeg12 ; libtiff
Elles ne sont peut-être pas toutes utiles selon les cas, mais au moins on les aura pour une prochaine fois. Si QGIS est déjà installé sur cette machine, il est possible que certaines soient déjà installées.
<br>
<br>
* Ouverture du shell OSGeo4W dans Windows en le recherchant "OSGeo4W Shell"

### Conversion
Pour convertir un ECW sans bande de transparence en Geotiff avec compression JPEG 90%
Après quelques tests, la compression à 60% à l'air d'un bon compromis entre qualité et poids. On aura tout de même un léger grain, si ça ne convient pas préférez le 70 ou 80 voire même 90, masi attention à la taille du fichier final (jusqu'a 2 fois plus lourd que l'ecw de base)
```
	gdal_translate -co COMPRESS=JPEG -co JPEG_QUALITY=90 -co PHOTOMETRIC=YCBCR -co TILED=YES -co BIGTIFF=YES -of GTiff C:/repertoire1/orthophoto.ecw C:/repertoire2/orthophoto.tiff
```

### Pyramidage
Attention, le paramètre JPEG_QUALITY_OVERVIEW doit être identique au paramètre JPEG_QUALITY de la conversion
```
	gdaladdo -r AVERAGE -ro --config JPEG_QUALITY_OVERVIEW 90 --config COMPRESS_OVERVIEW JPEG --config PHOTOMETRIC_OVERVIEW YCBCR --config INTERLEAVE_OVERVIEW PIXEL --config BIGTIFF_OVERVIEW YES -clean C:/repertoire2/orthophoto.tiff 2 4 8 16 32 64
```

Ajouter --config GDAL_CACHEMAX 1000 --config GDAL_MAX_DATASET_POOL_SIZE 1014 lors de la conversion et --config GDAL_CACHEMAX 1000 lors de la construction des pyramides.

Les options -co PHOTOMETRIC=YCBCR et --config PHOTOMETRIC_OVERVIEW YCBCR ne fonctionnent que sur des dalles sans bande transparente.
