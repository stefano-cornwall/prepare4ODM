# prepare4ODM
Geotag images using exiftool


exiftool -geotag=2016-05-07\ 19-14-51.log.gpx ~/data/drone/missions/160507/images/10405
for file in $(ls *.JPG) ; do gdalinfo $file  | grep Lat   >> DEGlat.txt ; done
for file in $(ls *.JPG) ; do gdalinfo $file  | grep Lon   >> DEGlon.txt ; done


cat DEGlon.txt  | awk 'NR % 2 == 1 ' | awk -F\= '{ gsub(/\(/,""); gsub(/\)/,""); print $2}' | awk '{print $1 "\xc2\xb0 " $2"\047 "$3" W"}' > deg_lon_format.txt

cat DEGlat.txt  | awk 'NR % 2 == 1 ' | awk -F\= '{ gsub(/\(/,""); gsub(/\)/,""); print $2}' | awk '{print $1 "\xc2\xb0 " $2"\047 "$3" N"}' > deg_lat_format.txt

paste -d"," deg_lat_format.txt deg_lon_format.txt  >latlonDeg.csv
