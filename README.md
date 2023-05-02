# Notebooks and Data for Scientific Computation - Spring 23

This is a collection of notebooks and data, which will be added to throughout the term.

## Notebooks

### Week 1

* Explore data on birth rates in the US: [Birth Data Exploration.ipynb](notebooks/Birth%20Data%20Exploration.ipynb)
* Start our introduction to sampling: [Intro to Sampling.ipynb](notebooks/Intro%20to%20Sampling.ipynb)

### Week 2

* Wrap up our introduction to sampling: [Intro to Sampling.ipynb](notebooks/Intro%20to%20Sampling.ipynb)
* Introduce Gaia data and explore the solar neighborhood: [Solar Neighborhood w Gaia.ipynb](notebooks/Solar%20Neighborhood%20w%20Gaia.ipynb)

### Week 3

* Wrap up [Intro to Sampling.ipynb](notebooks/Intro%20to%20Sampling.ipynb)
* Recall the Boltzmann distribution, use Ising model to understand the behavior of a ferromagnet at finite temperature: [Boltzmann and Ising (and some Metropolis).ipynb](notebooks/Boltzmann%20and%20Ising%20(and%20some%20Metropolis).ipynb)
* We made a quick-and-dirty observational HR diagram from our Gaia data set [Solar Neighborhood w Gaia.ipynb](notebooks/Solar%20Neighborhood%20w%20Gaia.ipynb)

### Week 4

* Introduce JAX and NumPyro for probabilistic model building and inference: [Intro to NumPyro.ipynb](notebooks/Intro%20to%20NumPyro.ipynb)
* Employ a mixture model in NumPyro to account for outliers in linear regression: [Modeling Outliers w NumPyro.ipynb](notebooks/Modeling%20Outliers%20w%20NumPyro.ipynb).

### Week 5

* Build a sequentially more complex model with `NumPyro` to make inferences from CO<sub>2</sub> concentrations in Mauna Loa, Hawaii.

## Data Provenance

### Exploring births in the US

US Birth data from the Social Security Administration, prepared by FiveThirtyEight.

[source](https://github.com/fivethirtyeight/data/tree/master/births)

This data can be with a wget command:

```bash
mkdir -p ../data
wget -qO ../data/US_births_2000-2014_SSA.csv https://raw.githubusercontent.com/fivethirtyeight/data/master/births/US_births_2000-2014_SSA.csv
```

### Solar Neighborhood w/ Gaia

We will use the [Gaia DR3 data release](https://gea.esac.esa.int/archive/) to explore the solar neighborhood. The data is available from the Gaia Archive. We will use the following query to get the data:

```sql
SELECT TOP 300000 phot_g_mean_mag+5*log10(parallax)-10 AS mg, bp_rp, parallax FROM gaiadr3.gaia_source
WHERE parallax_over_error > 10
AND parallax > 10
AND phot_g_mean_flux_over_error>50
AND phot_rp_mean_flux_over_error>20
AND phot_bp_mean_flux_over_error>20
AND phot_bp_rp_excess_factor < 1.3+0.06*power(phot_bp_mean_mag-phot_rp_mean_mag,2)
AND phot_bp_rp_excess_factor > 1.0+0.015*power(phot_bp_mean_mag-phot_rp_mean_mag,2)
AND visibility_periods_used>8
AND astrometric_chi2_al/(astrometric_n_good_obs_al-5)<1.44*greatest(1,exp(-0.4*(phot_g_mean_mag-19.5)))
```

### Synthetic data for linear regression

This data accompanies [Hogg, Bovy, and Lang (2010)](https://arxiv.org/abs/1008.4686).  It can be downloaded directly with

```bash
!wget -o ../data/data_yerr.dat https://raw.githubusercontent.com/davidwhogg/DataAnalysisRecipes/master/straightline/src/data_yerr.dat
```

### CO<sub>2</sub> Concentrations in Mauna Loa, Hawaii

Monthy-averaged CO<sub>2</sub> concentrations measured in Mauna Loa, Hawaii, hosted by the NOAA:

```bash
!wget -q ftp://aftp.cmdl.noaa.gov/products/trends/co2/co2_mm_mlo.txt -O ../data/co2_mm_mlo.txt
```