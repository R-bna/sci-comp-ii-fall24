# Notebooks and Data for Scientific Computation - Spring 23

This is a collection of notebooks and data, which will be added to throughout the term.

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
SELECT TOP 300000 phot_g_mean_mag+5*log10(parallax)-10 AS mg, bp_rp, parallax FROM gaiaedr3.gaia_source
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