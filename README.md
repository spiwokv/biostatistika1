# Biostatistika 1

```R
# Ja si "vygenerovat IQ":
iq <- rnorm(1, 100, 15)
iq   # 119.4084
# IQ je veličina, která má z definice normální rozdělení,
# průměr v populaci 100 a směrodatnou odchylku 15. Funkce
# rnorm vytvoří pořadovaný počet hodnot (první argument)
# náhodných čísel se zvoleným průměrem (druhý argument)
# a směrodatnou odchylkou (třetí argument). Opakované
# použití vede k novým náhodným číslům.

# Jak si vygenerovat vzorek 10 hodnot IQ:
iq10 <- rnorm(10, 100, 15)
iq10   # 67.76988  89.44135  97.87083 ...

# Jak spočítat výběrový průměr:
mean(iq10)  # 102.4597

# Jak spočítat výběrový směrodatnou odchylku:
sd(iq10)    # 17.79517

# Čím větší je vzorek, tím více se bude hodnota výběrového
# průměru blížit 100:
iq10 <- rnorm(10, 100, 15)
iq100 <- rnorm(100, 100, 15)
iq1k <- rnorm(1000, 100, 15)
iq10k <- rnorm(10000, 100, 15)
mean(iq10)   # 93.95456
mean(iq100)  # 100.85
mean(iq1k)   # 99.88719
mean(iq10k)  # 100.1484

# Čím větší je vzorek, tím více se bude hodnota směrodatné
# odchylky blížit 15:
sd(iq10)    # 17.60879
sd(iq100)   # 15.40315
sd(iq1k)    # 14.54683
sd(iq10k)   # 14.90575

# Představme si, že byl proveden klinický test léčiva,
# které má zvyšovat hodnotu IQ. Skupina 20 doborvolníků
# byla rozdělena na dvě skupiny po 10. Experimentální
# skupině bylo podáváno léčivo, kontrolní bylo podáváno
# placebo. Při generování vzodových hodnot budeme
# předpokládat, že léčivo skutečně zvyšuje IQ o 50 bodů.
# Výsledky můžeme porovnat t-testem:
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 150, 15)
t.test(ctrl, expr)

# 	Welch Two Sample t-test
# 
# data:  ctrl and expr
# t = -7.5896, df = 12.898, p-value = 4.154e-06
# alternative hypothesis: true difference in means is not equal to 0
# 95 percent confidence interval:
#  -64.18703 -35.72461
# sample estimates:
# mean of x mean of y 
#  94.29462 144.25043 

# Nulovou hypotézou je, že léčivo neovlivňuje IQ.
# Alternativní je, že IQ ovlivňuje. Výsledná p-hodnota
# 4.154e-06 je nižší než hraniční hodnota 0.05, proto
# zamítáme nulovou hypotézu, léčivo funguje.

# Výsledná p-hodnota je ovlivněna:
# 1. rozdílem průměrů (čím více bude léčivo ovlivňovat IQ, tím
#    snáze tento vliv prokážeme):
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 110, 15)
t.test(ctrl, expr)  # p = 0.1714
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 150, 15)
t.test(ctrl, expr)  # p = 1.319e-05
# 2. variabilitou veličiny (pokud by mělo IQ směrodatnou odchylku
#    1 místo 15, pak by bylo snažší odhalit menší ovlivnění IQ):
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 110, 15)
t.test(ctrl, expr)  # p = 0.4812
ctrl <- rnorm(10, 100, 1)
expr <- rnorm(10, 110, 1)
t.test(ctrl, expr)  # p = 5.555e-12
# 3. počtem vzorků (je snažší odhalit odchylku pokud máme hodně
#    měření):
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 110, 15)
t.test(ctrl, expr)  # p = 0.09545
ctrl <- rnorm(1000, 100, 15)
expr <- rnorm(1000, 110, 15)
t.test(ctrl, expr)  # p < 2.2e-16

# Pozor na možné desinterpretace p-hodnoty! Pokud bychom měli
léčivo, které mění IQ o 10 bodů a měli bychom 100 a 100
dobrovolníků, pak by byl test velmi přesvědčivý (p=3.332e-06)
ctrl <- rnorm(100, 100, 15)
expr <- rnorm(100, 110, 15)
t.test(ctrl, expr)  # 3.332e-06
x <- c(rep(1, 100)+0.1*rnorm(100), rep(2, 100)+0.1*rnorm(100))
# hodnoty jsou 100x 1 a 100x 2 s trochou šumu
plot(x, c(ctrl, expr), xlim=c(0,3))

# Jak je vidět z grafu, hodnoty v experimentální skupině
# jsou posunuté nahoru, ale prolínají se. Jak toto interpretovat?
# 1. Můžeme říci, že léčivo ovlivňuje IQ? Ano, je téměř vyloučeno,
#    že dosažený výsledek by byl dílem náhody.
# 2. Můžeme říci, že by lidstvo zinteligentnělo, kdyby užívali
#    léčivo? Ano, je téměř jisté, že by přibylo lidí s vyšším
#    IQ.
# 3. Můžeme říci, že by člověk který užívá léčivo je
#    inteligentnější než někdo, kdo léčivo neužívá? Ne.
#    Kdybychom vzali náhodného člověka z kontrolní a z
#    experimentální skupiny, pak by byla pravděpodobnost, že
#    uživatel léčiva je inteligentnější rovna 62 %:
sum(ctrl<expr)  # 62

# Pokud bychom vygenerovali data tak, aby platila nulová
# hypotéza (léčivo nefunguje), mělo by nám vyjít, že
# p-hodnota je vyšší než 0.05, tedy nezamítáme nulovou
# hypotézu, tedy nedokázali jsme, že léčivo funguje. Ani
# jsme neprokázali, že nefunguje!
ctrl <- rnorm(10, 100, 15)
expr <- rnorm(10, 100, 15)
t.test(ctrl, expr)

# 	Welch Two Sample t-test
# 
# data:  ctrl and expr
# t = 1.2672, df = 15.795, p-value = 0.2235
# alternative hypothesis: true difference in means is not equal to 0
# 95 percent confidence interval:
#  -4.957941 19.654297
# sample estimates:
# mean of x mean of y 
# 100.91404  93.56586 

# Pořád máme určitou pravděpodobnost (zde 5 %), že
# nesprávně zamítneme nulovou hypotézu. Když pokud
# opakujeme 100x, měl bychom vidět přibližně 5 příkladů
# nesprávného zamítnutí nulové hypotézy:
pvals <- c()
for(i in 1:100) {
  ctrl <- rnorm(10, 100, 15)
  expr <- rnorm(10, 100, 15)  
  pval <- t.test(ctrl, expr)$p.val
  pvals <- c(pvals, pval)
}
pvals  # 0.46402544 0.79416813 0.60474046 ...
sum(pvals < 0.05) # 5
```
