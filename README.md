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
```
