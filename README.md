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
