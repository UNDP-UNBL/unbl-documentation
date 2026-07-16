# How do I clip and export datasets? XYZ

Registered users on UN Biodiversity Lab are able to:

- Clip raster datasets to an area of interest and download them for use in a desktop GIS software--this function allows users to access the underlying data while avoiding the bandwidth and storage required to download and work with a global dataset;

- Download the boundary file of an area of interest as a GeoJSON for use in a desktop GIS software. 

<details class="unbl-video">
  <summary>▶️ Prefer video? Click here!</summary>

  <div class="video-wrapper">
    <iframe
      src="https://www.youtube-nocookie.com/embed/Ec1JfnYVFoY"
      title="UNBL tutorial"
      frameborder="0"
      allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
      allowfullscreen>
    </iframe>
  </div>
</details> 

To do this:

1.	Click on the 'PLACES' button and select your places of interest.

2.	Click on the ![](images/icons/ellipsis-icon.png){style="display: inline; width: 1em; height: 1em; width: 2em;"} icon to the right of the country’s name and click on 'Download GeoJSON' to download the boundary file for your area of interest, or click on 'Clip and Export Layers' if you want to clip and download a specific dataset. If picking the latter option, follow the additional steps 3 - 6 outlined below.

	![](images/en/image073.png)

3.  Type the name or select the data you want to download. If the data contains layers of multiple years, select the year you want to download. You have the option of downloading clipped layers in a GeoTIFF raster format or a PNG image file format.

4. Click download.

	- The selected data source will be clipped to the bounding box around the country.
	
	- There is a small buffer added to the bounding box, which will slightly enlarge the area of the clipped raster. This helps to ensure that any incongruities between the national boundary used in UNBL and the official national boundary file you may wish to use do not result in loss of data. This assumes that differences are potentially small. If this is not the case, please contact us at <support@unbiodiversitylab.org> for assistance.
	
	!!!Note
		If you are downloading GeoTIFFs, this is raw data and will not include styling information.

	![](images/en/image075.png)
	
5.	Access the downloaded .zip compressed file in your downloads folder once the download is complete. 

6.	The downloaded data can be opened in any GIS software for further analysis.

