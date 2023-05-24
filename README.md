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

* Build a sequentially more complex model with `NumPyro` to make inferences from CO<sub>2</sub> concentrations in Mauna Loa, Hawaii: [CO2 w NumPyro.ipynb](notebooks/CO2%20w%20NumPyro.ipynb).
* Introduce concepts and vocabulary for machine learning: [Intro to Machine Learning (w Gaia).ipynb](notebooks/Intro%20to%20Machine%20Learning%20(w%20Gaia).ipynb)
* Introduce logistic regression: [Logistic Regression.ipynb](notebooks/Logistic%20Regression.ipynb)

### Week 6

* Build and train a logistic regression classifier to identify quasars in SDSS observations: [Logistic Regression w SDSS.ipynb](notebooks/Logistic%20Regression%20w%20SDSS.ipynb)
* Extend our use of logistic regression to handle more than two classes: [Multiclass Classification.ipynb](notebooks/Multiclass%20Classification.ipynb)

### Week 7

* Wrap up our multi-class classification work from Week 6
* Introduce neural networks: [Intro to Neural Networks.ipynb](notebooks/Intro%20to%20Neural%20Networks.ipynb)
* Introduce `Flax` and use a dense neural network layer to perform linear regression: [Intro%20to%20Flax.ipynb](notebooks/Intro%20to%20Flax.ipynb)
* Use `Flax` to construct a dense neural network for classifying handwritten digits: [Dense Neural Network on MNIST Digits.ipynb](notebooks/Dense%20Neural%20Network%20on%20MNIST%20Digits.ipynb)

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

### Logistic Regression Synthetic Data

To introduce logistic regression we make use of some data used by Jordi Warmenhoven in their [Coursera Machine Learning course](https://github.com/JWarmenhoven/Coursera-Machine-Learning). 
```bash
!wget https://raw.githubusercontent.com/JWarmenhoven/Coursera-Machine-Learning/master/notebooks/data/ex2data1.txt -O ../data/ex2data1.txt
!wget https://raw.githubusercontent.com/JWarmenhoven/Coursera-Machine-Learning/master/notebooks/data/ex2data2.txt -O ../data/ex2data2.txt
```

### SDSS Quasars

This is data collected by the Sloan Digital Sky Survey (SDSS) relating to quasars. The catalogs we'll be using are part of [PSU's astrostatistics data sets](https://astrostatistics.psu.edu/datasets/index.html).  We need three separate files, separated by spectroscopically confirmed classifications.

Spectroscopically confirmed stars:
```bash
!wget -q --no-check-certificate -O ../data/SDSS_stars.csv https://astrostatistics.psu.edu/MSMA/datasets/SDSS_stars.csv
```

white dwarfs:
```bash
!wget -q --no-check-certificate -O ../data/SDSS_wd.csv https://astrostatistics.psu.edu/MSMA/datasets/SDSS_wd.csv
```

and quasars:
```bash
!wget -q --no-check-certificate -O ../data/SDSS_quasar.dat https://astrostatistics.psu.edu/datasets/SDSS_quasar.dat
```

More info on the dataset can be found [here](https://astrostatistics.psu.edu/datasets/SDSS_quasar.html).

### M4

We make use of two separate data products from the Gaia collaboration. First is a cluster catalog [here](http://cdsarc.u-strasbg.fr/ftp/J/A+A/616/A12/files/NGC6121-1.dat), which is associated with [this paper](https://www.aanda.org/articles/aa/abs/2018/08/aa32698-18/aa32698-18.html) looking at the kinematics of many globular clusters.  The full data release associated with the paper can be found [here](http://cdsarc.u-strasbg.fr/viz-bin/qcat?J/A+A/616/A12), and includes tables of members identified for each cluster they studied. This can be downloaded directly with:
```bask
wget http://cdsarc.u-strasbg.fr/ftp/J/A+A/616/A12/files/NGC6121-1.dat -O ../data/NGC6121-1.dat
```

Second, we use `m4_gaia_source.csv`, which was pulled from the Gaia data archive with the following query:
```sql
SELECT TOP 1000000 gaia_source.designation,gaia_source.source_id,gaia_source.ra,gaia_source.dec,gaia_source.parallax,gaia_source.parallax_error,gaia_source.parallax_over_error,gaia_source.pm,gaia_source.pmra,gaia_source.pmra_error,gaia_source.pmdec,gaia_source.pmdec_error,gaia_source.astrometric_n_good_obs_al,gaia_source.astrometric_chi2_al,gaia_source.visibility_periods_used,gaia_source.phot_g_mean_flux_over_error,gaia_source.phot_g_mean_mag,gaia_source.phot_bp_mean_flux_over_error,gaia_source.phot_bp_mean_mag,gaia_source.phot_rp_mean_flux_over_error,gaia_source.phot_rp_mean_mag,gaia_source.phot_bp_rp_excess_factor,gaia_source.bp_rp,gaia_source.radial_velocity,gaia_source.radial_velocity_error
FROM gaiadr3.gaia_source 
WHERE 
CONTAINS(
	POINT('ICRS',gaiadr3.gaia_source.ra,gaiadr3.gaia_source.dec),
	BOX('ICRS',246,-26.5,3,3)
)=1
```