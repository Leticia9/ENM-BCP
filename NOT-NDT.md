# ENM-BCP
#Niche Overlap Test and Niche Divergence Test
#Leticia Cab-Sulub
#Centro de Investigaciones Biológicas del Noroeste
#México

library(humboldt)
library(raster)

setwd("E:/Documents/")

#load layers 
capas <- list.files("sp1_n/",pattern = "*.asc$",full.names = TRUE)
capas <- stack(capas)

capas.s <- list.files("sp1_s/",pattern = "*.asc$",full.names = TRUE)
capas.s <- stack(capas.s)

#we need to convert a layer of each population into a point file
bio1 = raster("sp1_n/bio1.asc")
bio1.s = raster("sp1_s/bio1.asc")

#extract points
env.points<-rasterToPoints(bio1, fun=NULL, spatial=FALSE)
env.points.2<-rasterToPoints(bio1.s, fun=NULL, spatial=FALSE)

#we filter the points
env.sampling.res<-humboldt.occ.rarefy(env.points, colxy = 1:2, rarefy.dist = 10,  rarefy.units = "km", run.silent.rar = F)
env.sampling.res.2<-humboldt.occ.rarefy(env.points.2, colxy = 1:2, rarefy.dist = 10,  rarefy.units = "km", run.silent.rar = F)

#subset only the x and y data
env.sampling.res<- env.sampling.res[,1:2]
env.sampling.res.2<- env.sampling.res.2[,1:2]

##Extract values to points from all rasters
rast_capas <- data.frame(extract(capas, env.sampling.res))
rast_capas.s <- data.frame(extract(capas.s, env.sampling.res.2))

##merge sampled data to input
Env1<-cbind(env.sampling.res,rast_capas)
Env2<-cbind(env.sampling.res.2,rast_capas.s)

##save the file as '.csv'
write.csv(Env1, file = "NOTyNDT/Env_sp1N.csv")
write.csv(Env2, file = "NOTyNDT/Env_sp1S.csv")

