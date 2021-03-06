[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.43-blue.svg)](https://doi.org/10.25663/bl.app.43)

# app-tractanalysisprofiles
This service computes profiles of tensor (i.e. AD, FA, MD, RD) and/or NODDI (i.e. ICVF, ISOVF, OD) measures, and their inverse measures and corresponding standard deviations, over a user-specified number of nodes along segmented white matter fascicles. Based on user-input, it computes these profiles using either dtiComputeDiffusionPropertiesAlongFG_sd from Vistasoft (, which weights streamlines based on their distance away from a central core of the fascicle (i.e. 'volume based'), or Compute_FA_AlongFG, which does not weight the streamlines and is appropriate for testing models involving streamline/fiber corssings (i.e. 'fiber based'). EPS and PNG images of these profiles are created for the measures, and a .csv file is outputted containing all the measures and standard deviations for each segmented fascicle.

### Authors
- Brad Caron (bacaron@iu.edu)
- Lindsey Kitchell (kitchell@iu.edu)

### Contributors
- Soichi Hayashi (hayashi@iu.edu)
- Franco Pestilli (franpest@indiana.edu)

### Funding
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)

## Running the App 

### On Brainlife.io

You can submit this App online at [https://doi.org/10.25663/bl.app.43](https://doi.org/10.25663/bl.app.43) via the "Execute" tab.

### Running Locally (on your machine)

1. git clone this repo.
2. Inside the cloned directory, create `config.json` with something like the following content with paths to your input files.


* Running with tensor

```json
{
        "fa": "./testdata/tensor/fa.nii.gz",
        "md": "./testdata/tensor/md.nii.gz",
        "rd": "./testdata/tensor/rd.nii.gz",
        "ad": "./testdata/tensor/ad.nii.gz",
        "afq": "./testdata/wmc/classification.mat",
        "tck": "./testdata/tck/track.tck",
        "numnodes": 100,
        "fiberbased": false
}
```

* Running with noddi

```json
{
        "icvf": "./testdata/noddi/icvf.nii.gz",
        "isovf": "./testdata/noddi/isovf.nii.gz",
        "od": "./testdata/noddi/od.nii.gz",
        "afq": "./testdata/wmc/classification.mat",
        "tck": "./testdata/tck/track.tck",
        "numnodes": 100,
        "fiberbased": false
}```

### Sample Datasets

You can download sample datasets from Brainlife using [Brainlife CLI](https://github.com/brain-life/cli).

```
npm install -g brainlife
bl login
mkdir input
bl dataset download 5b96bdce059cf900271924fb && mv 5b96bdce059cf900271924fb input/tensor #for tensor mode
bl dataset download 5b96bdba059cf900271924fa && mv 5b96bdba059cf900271924fa input/noddi #for noddi mode
bl dataset download 5b96bdd2059cf900271924fc && mv 5b96bdd2059cf900271924fc input/wmc
bl dataset download tdb.. && mv tdb.. input/tck
```

3. Launch the App by executing `main`

```bash
./main
```

## Output

The two main outputs of this App are folders called "images" and "tractprofile". The "images" folder contains .eps and .png images of the tract profiles for every measure and fascicle. The "tractprofile" folder contains a .csv file for every fascicle. The .csv file follows this layout:

```
ad_1 (measure data) ad_2 (measure std) fa_1 (measure data) fa_2 (measure std) md_1 (measure data) md_2 (measure std) rd_1 (measure data) rd_2 (measure std) ad_inverse_1 (measure data) ad_inverse_2 (measure std) fa_inverse_1 (measure data) fa_inverse_2 (measure std) md_inverse_1 (measure data) md_inverse_2 (measure std) rd_inverse_1 (measure data) rd_inverse_2 (measure std) icvf_1 (measure data) icvf_2 (measure std) isovf_1 (measure data) isovf_2 (measure std) od_1 (measure data) od_2 (measure std) icvf_inverse_1 (measure data) icvf_inverse_2 (measure std) isovf_inverse_1 (measure data) isovf_inverse_2 (measure std) od_inverse_1 (measure data) od_inverse_2 (measure std)
```

#### Product.json
The secondary output of this app is `product.json`. This file allows web interfaces, DB and API calls on the results of the processing. 

### Dependencies

This App requires the following libraries when run locally.

  - singularity: https://singularity.lbl.gov/
  - VISTASOFT: https://github.com/vistalab/vistasoft/
  - SPM 8: https://www.fil.ion.ucl.ac.uk/spm/software/spm8/
  - jsonlab: https://github.com/fangq/jsonlab.git
  
#### References
(v0.1)  —  Yeatman J.D., Dougherty R.F., Myall N.J., Wandell B.A., Feldman H.M. (2012). Tract Profiles of White Matter Properties: Automating Fiber-Tract Quantification. PLoS One.

(v1.1)  —  Yeatman J.D., Wandell B.A., Mezer A. (2014). Lifespan Maturation and Degeneration of Human Brain White Matter. Nature Communications.

(v1.2)  —  Yeatman J.D., Weiner K.S., Pestilli F., Rokem A., Mezer A., Wandell B.A. (2014). The Vertical Occipital Fasciculus: A Century of Controversy Resolved By In Vivo Measurements. PNAS.
