### Pediatric buccal DNAm clock 
(currently under review as of June 3,2019)
#### Notes: this is a DNAm predictor for age from buccal epithelial swabs. It's intended purpose is buccal samples from individuals aged 0-20 years old. Highlighy recommend controlling for esimtaed buccal cell proportion when comparing the age acceleration residuals (obtained from regressing predicted age onto chronological age and extracting residuals) with some variable. If you have any questions please contact Lisa McEwen (lmcewen@bcchr.ca).
#### Betas should be probe filtered and background substracted and colour corrected. Original training DNAm data was probe filtered, sample outliers removed, probe NAs were imputed using impute.knn and probe type design differences were adjusted using of a modified version of BMIQ (code can be accessed from Steve Horvaths website).  

### IMPORTANT: You may use this code with acknowledgement that you may not publish any result from this code prior to the publication of the original article which is currently under review. 

Assume a beta data set 'dat0' where the rows of 'dat0' correspond to the CpGs and the first column contains the CpG identifier. The remaining columns correspond to buccal samples.

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
DNAmAgeBuccal=as.numeric(anti.trafo(datClock[1,2]+as.numeric(as.matrix(datMethClock)%*%
as.numeric(datClock[,2][-1]))))
```
