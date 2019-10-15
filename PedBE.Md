
### The PedBE (Pediatric-Buccal-Epigenetic) Clock 

[Link to paper](https://www.pnas.org/content/early/2019/10/09/1820843116.short?rss=1)

#### Notes: this is a DNAm predictor for age from buccal epithelial swabs. It's intended purpose is buccal samples from individuals aged 0-20 years old. Highlighy recommend controlling for esitimated buccal cell proportion when comparing the age acceleration residuals (obtained from regressing predicted age onto chronological age and extracting residuals) with variable of interest. 

#### Betas should be probe filtered, background substracted, and colour corrected. Original training DNAm data was probe filtered, sample outliers removed, probe NAs were imputed using impute.knn and probe type design differences were adjusted using of a modified version of BMIQ (code can be accessed from Steve Horvaths website). Please see paper for more details.

Assume a beta data set 'dat0' where the rows of 'dat0' correspond to the CpGs and the first column contains the CpG identifier. The remaining columns correspond to sample identifiers.

### PedBE Age 
```{r}
datM=t(dat0[,-1])
colnames(datM)=as.character(dat0[,1])
anti.trafo= function(x,adult.age=20) {
ifelse(x<0, (1+adult.age)*exp(x)-1, (1+adult.age)*x+adult.age) }
datClock=read.csv("datcoefInteresting94.csv") #based on ALL samples
selectCpGsClock=is.element(dimnames(datM)[[2]],
as.character(datClock[,1][-1]))
datMethClock0=data.frame(datM[,selectCpGsClock])
datMethClock= data.frame(datMethClock0[
as.character(datClock[,1][-1])])
PedBE_age=as.numeric(anti.trafo(datClock[1,2]+as.numeric(as.matrix(datMethClock)%*%
as.numeric(datClock[,2][-1]))))
```

 ***
 
<img src="https://github.com/kobor-lab/Public-Scripts/blob/master/ubc-logo.png" width="200" style="float: left; height: 15em; margin-right: 1%; margin-bottom: 0.5em;"> <img src="https://github.com/kobor-lab/Public-Scripts/blob/master/gjvlh3ws.jpg" width="200" style="float: right; height: 15em; margin-left: 1%; margin-bottom: 0.5em;">
