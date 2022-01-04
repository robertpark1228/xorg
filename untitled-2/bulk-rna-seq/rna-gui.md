# RNA-GUI 분석툴 메뉴얼

1\) TCC-GUI (WEB/SHINY BASE)

GITHUB : [https://github.com/swsoyee/TCC-GUI](https://github.com/swsoyee/TCC-GUI)

INSTALL :  Pre-installation

Make sure that you have already installed those packages in your environment.

`shiny`, `shinydashboard`, `shinyWidgets`, `plotly`, `dplyr`, `TCC`, `DT`, `heatmaply`, `rmarkdown`, `data.table`, `tidyr`, `RColorBrewer`, `utils`, `knitr`, `cluster`, `shinycssloaders`, `shinyBS`, `renv`, `MASS`.

If any package is missing, Please run the following command in your [`RStudio`](https://www.rstudio.com) and it will install all packages automatically.

```
# Check "BiocManager"
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Package list
libs <- c("shiny", "shinydashboard", "shinyWidgets", "plotly", "dplyr", "DT", "heatmaply", "tidyr","utils","rmarkdown","data.table","RColorBrewer", "knitr", "cluster", "shinycssloaders", "shinyBS", "renv", "MASS", "TCC")

# Install packages if missing
for (i in libs){
  if( !is.element(i, .packages(all.available = TRUE)) ) {
     BiocManager::install(i, suppressUpdates=TRUE)
  }
}
```

#### Start the App

Run the following command to launch `TCC-GUI` in your local environment, then it will download `TCC-GUI` automatically from github and launch.





```
shiny::runApp("[TCC-GUI 다운받은 폴더]", host = 내서버IP, orprfsdfsdfsdff)
```

*
*
*
*
*
*
* ![](<../../.gitbook/assets/image (304).png>)



