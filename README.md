# save_as_multiple_plots
save as multiple plots

```
library(pryr)

save_as_multiple_formats<-function(filename,width,height){
  setEPS()
  postscript(paste0(filename,".eps"),width=width,height=height)
  a_plot
  dev.off()
  
  pdf(paste0(filename,".pdf"),width=width,height=height)
  a_plot
  dev.off()
  
  jpeg(paste0(filename,".jpg"),width=width,height=height,units="in",res=600)
  a_plot
  dev.off()
  
  # svg(paste0(filename,".svg"),width=width,height=height)
  # a_plot
  # dev.off()
}

a_plot%<a-%{
  ...
}

save_as_multiple_formats("test",width=10,height=20)
```
## Multiple-page JPG
```
library(pryr)

save_as_multiple_formats<-function(filename,width,height,multiple=F){
  setEPS()
  postscript(paste0(filename,".eps"),width=width,height=height,family="Times")
  a_plot
  dev.off()
  
  pdf(paste0(filename,".pdf"),width=width,height=height)
  a_plot
  dev.off()
  
  jpeg(paste0(filename,ifelse(multiple,".%02d",""),".jpg"),width=width,height=height,units="in",res=600)
  a_plot
  dev.off()
  
  # svg(paste0(filename,".svg"),width=width,height=height)
  # a_plot
  # dev.off()
}
```
## Rename

```
library(tidyverse)

dir_output<-"output"
dir_output_cleaned<-"output_cleaned"
all_fmt<-c("pdf","eps","jpg")
dir.create(dir_output_cleaned)
for(fmt in all_fmt)dir.create(file.path(dir_output_cleaned,fmt))

replace_names<-function(original){
  replaced<-dplyr::case_when(
    original=="xxx"~"yyy")
  return(replaced)
}

files_output<-list.files("output")
df_files<-data.frame(
  file=files_output,
  original=fs::path_ext_remove(files_output),
  ext=fs::path_ext(files_output))%>%
  mutate(
    title=
      ifelse(
        grepl("\\.",original),
        gsub("^([^\\.]*)(\\.\\d{2})$","\\1",original),
        original),
    page=
      ifelse(
        grepl("\\.",original),
        gsub("^([^\\.]*)\\.(\\d{2})$","\\2",original),
        NA_character_))%>%
  mutate(
    replaced=replace_names(title))%>%
  mutate(
    from=file.path(dir_output,file),
    to=file.path(dir_output_cleaned,ext,paste0(replaced,ifelse(is.na(page),"",paste0(".",page)),".",ext)))

for(fmt in all_fmt){
  file.copy(
    from=df_files%>%filter(!is.na(replaced),ext==fmt)%>%pull(from),
    to=df_files%>%filter(!is.na(replaced),ext==fmt)%>%pull(to),
    overwrite=T)
}
```
