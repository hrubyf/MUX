#  Map User Experience (MUX)
[Download RStudio](https://rstudio.com/products/rstudio/download/)
## Download der verwendeten Datensätze
https://drive.google.com/drive/folders/1BwxvCa-EckDScO1nfrugDHN9q4C6e5lB?usp=sharing
## Videotutorials | Statistische Datenauswertung
* [Aufbereitung und Prüfung der Daten](https://drive.google.com/file/d/1WHw_QzTLlgs3m8Rkjjl6maCz6rl9AHpd/view?usp=sharing)
* [Datenanalyse unter Annahme normalverteilter Daten](https://drive.google.com/file/d/1bHJHVFmlzQOXA5rHqfsyeFF0IqitNKBP/view?usp=sharing)
* [Datenanalyse unter Annahme nicht-normalverteilter Daten](https://drive.google.com/file/d/1RuCm685RAviR9bGiegqO5l1aVGSVyLUH/view?usp=sharing)
## R-snippets | Aufbereitung und Prüfung der Daten
a) Daten in RStudio laden:
```
original <- read.csv("geomap_O.csv", header=T)
```
b) Um sich einen ersten Überblick über die Daten zu verschaffen:
```
head(original)
summary(original)

boxplot(original)
plot(original)
```
c) Auf Normalverteilung testen
```
original_fence <- read.csv("geomap_O_fence.csv", header=T)

shapiro.test(original_fence$O1)

hist(original_fence$O1,main="Original",xlab="RT - milliseconds", ylab="Häufigkeit", border="light blue",col="blue",las=1)
```
## Datenanalyse unter Annahme normalverteilter Daten (parametrische Tests)
d) Fragen eines Legendendesigns in Bezug auf die Antwortzeit miteinander vergleichen
```
original <- read.csv("geomap_O_fence_list.csv", header=T)

anova_o <- aov(original$RT ~ original$type)
summary(anova_o)

TukeyHSD(anova_o)
```
e) Fragen verschiedener Legendendesigns in Bezug auf die Antwortzeit miteinander vergleichen
```
OvsF <- read.csv("geomap_OvsF.csv", header=T)

t.test(OvsF$F2, OvsF$O2, mu=0, alternative = "two.sided", var.eq = F, paired = F, conf.level = 0.95)
```
## Datenanalyse unter Annahme nicht-normalverteilter Daten (nicht-parametrische Tests)
f) Fragen verschiedener Legendendesigns in Bezug auf die Antwortzeit miteinander Vergleichen
```
OvsF_np <- read.csv("geomap_OvsF_np.csv", header=T)

wilcox.test(OvsF_np$F2, OvsF_np$O2, alternative = "two.sided", paired = F, conf.level = 0.95)
```
