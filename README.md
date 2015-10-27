# R_load_ncdf

Metadata for GMAO netcdf files:
  LWGAB = longwave absorbed at the ground surface = LWin
  LWGEM = longwave emitted at the ground surface = LWout
  LWGNT = LWGAB - LWGEM
  SWGDN = incoming shorwave radiation at the ground
  
Each variable has 23 grids for a given date, one for each hour.
Band 1 is for 12midnight to 1pm, etc.

```R
indir = "G:/large_datasets/USA/california/modis_fluxtower_sites/GMAO/"  # Directory with a list of ncdf files
patt="nc"
flist <- list.files(indir.sw,pattern = patt)
#  Load one file from the list
x=1  # Index to the list of files
blw.in.11am = raster(flist[x], band=19, varname = "LWGAB")

#  band=19 indicates the time out of 23 bands that you want.
#  Band 19 is 19:00 (7pm) UTC, or 11am Pacific Standard Time.
#  For summer (PDT), 11am PDT is 6pm UTC (18), or band 18
blw.out.11am = raster(flist[x], band=19, varname = "LWGAB")
bsw.11am = raster(flist[x], band=19, varname = "SWGDN")

dates.swlw = strptime(substr(file.list.swlw.sub[x],37,45),format="%Y%m%d") # The values 37 and 45 may need to be changed
  # to extract the correct dates.  Filenames for the above code are, for example:
  #  "MERRA300.prod.assim.tavg1_2d_rad_Nx.20100101.hdf.nc", so 20100101 is from 37 to 45
yyyyjjj.swlw = as.numeric(format(dates.swlw,"%Y%j"))  # Gives the year and julian date of the ncdf file

```R
