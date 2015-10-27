# R_load_ncdf

Metadata for GMAO netcdf files:
  LWGAB = longwave absorbed at the ground surface = LWin
  LWGEM = longwave emitted at the ground surface = LWout
  LWGNT = LWGAB - LWGEM
  SWGDN = incoming shorwave radiation at the ground
  
Each variable has 24 grids for a given date, one for each hour.

Band 1 is for 12midnight to 1pm UTC on Day 1 (= 5 pm PST on Day 0
  Midnight in San Diego (PST) is 8am UTC.
  Midnight in San Diego (PDT) is 7am UTC.
  
```R
indir = "G:/large_datasets/USA/california/modis_fluxtower_sites/GMAO/"  # Directory with a list of ncdf files
patt="nc"
flist <- list.files(indir.sw,pattern = patt)
#  Load one file from the list
x=1  # Index to the list of files
blw.in.11am = raster(flist[x], band=19, varname = "LWGAB")
```
band=19 indicates the time out of 23 bands that you want.
Band 19 is 19:00 (7pm) UTC, or 11am Pacific Standard Time.
For summer (PDT), 11am PDT is 6pm UTC (18), or band 18
```R
blw.out.11am = raster(flist[x], band=19, varname = "LWGAB")
bsw.11am = raster(flist[x], band=19, varname = "SWGDN")
dates.swlw = strptime(substr(file.list.swlw.sub[x],37,45),format="%Y%m%d") 
  # The values 37 and 45 may need to be changed depending on the file name
  # to extract the correct dates.  Filenames for the above code are, for example:
  #  "MERRA300.prod.assim.tavg1_2d_rad_Nx.20100101.hdf.nc", so 20100101 is from 37 to 45
yyyyjjj.swlw = as.numeric(format(dates.swlw,"%Y%j"))  # Gives the year and julian date of the ncdf file

plot(blw.out.11am)
```
To get 24 hour mean radiation values for, e.g. San Diego for e.g. January 1, 2010:
  PST:  Take bands 8-24 from January 1, 2010 and bands 1-7 from January 2, 2010.
  PDT:  Bands 7-24 from January 1, 2010 and bands 1-6 from January 2, 2010.

For PST:
```R
bsw.1 = stack(flist[x], bands=c(8:24), varname = "SWGDN")
bsw.2 = stack(flist[x+1],bands=c(1:7), varname = "SWGDN")
bsw.12 = stack(bsw.1,bsw.2)
bsw.24hr = round(mean(bsw.12),1)
```

For PDT:
```R
bsw.1 = stack(flist[x], bands=c(7:24), varname = "SWGDN")
bsw.2 = stack(flist[x+1],bands=c(1:6), varname = "SWGDN")
bsw.12 = stack(bsw.1,bsw.2)
bsw.24hr = round(mean(bsw.12),1)
```

