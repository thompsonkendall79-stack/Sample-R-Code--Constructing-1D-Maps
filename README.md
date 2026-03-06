# Sample-R-Code--Constructing-1D-Maps
This is a project description of an R markdown script I wrote for mapping out the various One Medical Amazon locations in the United States. 

```{r}

#Load Required Packages
install.packages(c("tidygeocoder", "readxl", "dplyr", "ggplot2", "geosphere"))
library(tidygeocoder)
library(readxl)
library(dplyr)
library(ggplot2)
library(geosphere)c
install.packages("ggrepel")
library(ggrepel)
install.packages("leaflet")
library(leaflet)

#
geocoded_df <- read.csv("C:/Users/Thomp/OneDrive/ITEC-300 R Files/clinics_pharmacies_geocoded.csv")

clinics <- geocoded_df %>%
  filter(Pharmacy %in% c("PA00_BERWICK", "PA00_BLOOMSBURG")) %>%
  mutate(
    Type = "Clinic",
    Popup = paste0(
      "<b>", Pharmacy, "</b><br>",
      Street.Address, "<br>",
      City, ", ", State, " ", Zipcode
    )
  )

pharmacies <- geocoded_df %>%
  filter(!(Pharmacy %in% c("PA00_BERWICK", "PA00_BLOOMSBURG"))) %>%
  mutate(
    Type = "Pharmacy",
    Popup = paste0(
      "<b>", Pharmacy, "</b><br>",
      Street.Address, "<br>",
      City, ", ", State, " ", Zipcode
    )
  )




leaflet() %>%
  addTiles() %>%
  addCircleMarkers(
    data = clinics,
    lng = ~Longitude, lat = ~Latitude,
    color = "red", radius = 6,
    label = ~Pharmacy,
    popup = ~Popup,
    group = "Clinics"
  ) %>%
  addCircleMarkers(
    data = pharmacies,
    lng = ~Longitude, lat = ~Latitude,
    color = "darkblue", radius = 5,
    label = ~Pharmacy,
    popup = ~Popup,
    group = "Pharmacies"
  ) %>%
  addLayersControl(
    overlayGroups = c("Clinics", "Pharmacies"),
    options = layersControlOptions(collapsed = FALSE)
  )
```
