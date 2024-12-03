# Globular Cluster Member Identification: NGC 6397

NGC 6397 is the second closest globular cluster to Earth (second to M4).  Similar to our exploration of M4 using Gaia, we're going to make use of a catalog of confidently identified stellar members of the cluster to train classifiers capable of making such classifications from Gaia observations.

**For this final project, complete the notebook found in the `notebooks` directory of this repository.**

## Catalog Data
The catalog, originally pulled from [here](http://cdsarc.u-strasbg.fr/ftp/J/A+A/616/A12/), can be found in `data/NGC6397-1.dat`.

From the Gaia archive consider all objects in a 2 deg x 1.5 deg box centered on the cluster.  The query below was used to generate `data/gaia-NGC6397-neighborhood.csv`.

```sql
SELECT TOP 500000 gaia_source.source_id,gaia_source.ra,gaia_source.dec,gaia_source.parallax,gaia_source.parallax_error,gaia_source.pm,gaia_source.pmra,gaia_source.pmra_error,gaia_source.pmdec,gaia_source.pmdec_error,gaia_source.phot_g_mean_mag,gaia_source.phot_bp_mean_mag,gaia_source.phot_rp_mean_mag,gaia_source.bp_rp,gaia_source.radial_velocity,gaia_source.radial_velocity_error
FROM gaiadr3.gaia_source 
WHERE 
CONTAINS(
	POINT('ICRS',gaiadr3.gaia_source.ra,gaiadr3.gaia_source.dec),
	BOX('ICRS',265.17,-53.68,2,1.5)
)=1
```

## Isochrones

The [MIST isochrone interpolator](http://waps.cfa.harvard.edu/MIST/interp_isos.html) was used to produce an isochrone based on known properties of NGC 6397 (metalicity, reddening due to dust, etc.), and saved `BP-RP`, `Gaia_G_EDR3=phot_g_mean_mag` (with distance modulus already applied), and phase (indicating stellar evolutionary phase) to `data/NGC6397_iso.csv`.