Note restrictive license below

### Brainstem Navigator v0.9 (aka Bianciardi atlas)

The Bianciardi atlas is an ongoing effort to map brainstem nuclei in vivo using high resolution multimodal MR. T1, T2 and diffusion fractional anisotrophy data is collected at 7T and structures are individually segmented in small samples of participants (10-20). Participants are aligned to the FSL standard space template (most likely MNI152NLin6Asym, but there's a slight possibility it's MNI152NLin6Sym). A probablistic map is created based on overlap of the sample in this standard space. This project spans a number of publications over the past decade and remains ongoing, but is already at a sufficiently advanced stage to be useful. Bianciardi 2015, the first paper in this series, is a good reference for details on methods.

Technically this folder does not provide the atlas, due to licensing restrictions that prevent us from redistributing the ROIs (see copyright notice below). Instead it contains the code needed to automatically download and reformat these ROIs to be compatable with canlab tools and multiple MNI152 spaces of interest (initially MNI152NLin6Sym space and MNI152NLin2009cAsym, the fmriprep 20.2.3 default, see methods below, but could be expanded). In this sense it draws from the approach taken by various linux distros with respect to problematic software like some oracle, adobe, google, mozilla, etc. products. A .gitignore file exists to prevent the generated files from being uploaded back to the repo. Please do not modify this so long as the license below remains in effect (check Bianciardi's Brainstem Imaging Lab website for updates).

This atlas is provided in two parcellations, one with some subnuclear divisions ("fine" scale) and a lower resolution atlas. The fine atlas probably excedes the spatial precision of contemporary imaging in the wager lab, but may be useful when working with specialty datasets, as a reference or as technology evolves, so it is provided as well. Note, the resolution in question here is not the resolution of the NIFTI image, but rather of the parcellation. The NIFTI images in both cases are 1x1x1mm resolution.

In fact, you should be aware that many of these regions are extremely small, even in the default, non fine scale atlas. In this form it is most useful as a way of asigning labels to activation maps you may have or otherwise being used as a reference. If you need to define seed regions or targets you probably will want to try dilating some of these regions to have a more practical volume to work with.

### Data formatting

The nuclei have obscure names and in many cases multiple names. We have shorthand names in the atlas.labels field, but longer descriptions are also available in the atlas.label_description field. Further, a select number of regions are not in fact brainstem regions, specifically LGN, MGN and subthalamic nucleaus. The macroscopic structure is indicated by atlas.labels_2.

### Evaluation of atlas quality

The standard for brainstem nuclei in the Wager lab was established by CANLab 2018 (which predates this atlas). In CANLab2018 coordinates were aggregated from multiple studies, mostly pain studies, which reported task evoked responses in particular nuclei. Most of these studies were conducted between 2005-2010, and would be considered archaic by today's standards, but they had the advantage of linking anatomy with function or sometimes behavior for validation. The Bianciardi atlas does not do this for the most part (although see Bianciardi 2016). Consequently you should approach these regions with some skepticism. However, the atlas has thus far held up under my own scrutiny.

Some of the regions here disagree with those of CANLab2018. The raphe nuclei present an opportunity for robust comparison, since there are several of them distributed along the rostro-caudal axis of the brainstem and most express 5-HT transporter and 5-HT1a autoreceptors, for which we have multiple PET tracer maps available (Hansen et al, 2022). The viability of identifying these nuclei by 5-HT transporter ligand was established in a series of papers which identified 3-amin-4-(2-dimethylaminomethylphenylthio) benzonitrile (DASB) as an effective ligand (Son Y et al. 2012 & Son Y et al. 2014). Hansen et al. provides two such maps and a third using a second 5-HT transporter ligand MADAM. Additionally, serotonin release appears regulated by 5-HT1a autoreceptors (Garcia-Garcia 2014), for which Hansen provides two more PET contrast maps using two different radioligands, offering a second independent method of validating the raphe nuclei. We can overlay the raphe parcels from the Bianciardi and CANLab2018 atlases onto the PET tracer maps to evaluate their relative accuracy.

Bianciardi offers the following raphe nuclei,
* Caudal-rostral linear raphe (CLi_RLi) (missing from canlab 2018)
* Dorsal raphe (DR)
* Median Raphe (MnR)
* Raphe Magnus (RMg)
* Raphe obscurus (ROb)
* Raphe pallidus (RPa) (missing from canlab2018)

Meanwhile CANLab2018 offers these,
* Dorsal raphe (Dorsal raphe DR)
* Median Raphe (Median Raphe MR R)
* Raphe Nucleus Centralis Superior (NCS B6 B8)
* Raphe Magnus (NRM)
* Medullary Raphe aka Raphe pallidus
* Nucleus raphe pontis (NRP B5) (missing from Bianciardi)

The first thing to notice is that they don't have identical coverage. Some ROIs are only in one but not the other. The other thing to notice is that CANLab2018 has both Median Raphe and Raphe Nucleus Centralis Superior which is a subdivision of the Median Raphe, but is suspect. Additionally the latter is associated with two nucleu, B6/B8. The B system is an independent numbering system for the Raphe to help distinguish them given redundant naming conventions, but this single nucleus is associated with two raphe nuclei. This might be fine for a nucleus with well defined boundaries, but the CANLab2018 ROIs are coordinate based and a 3 voxel sphere was simply drawn at those coordinates, so no meaningful subdivisions are possible.

How do these atlases compare when overlaid on the pet tracer data? All pet tracer overlays are available in the tracer_overlays directory of this folder, but I want to highlight three key ones. All overlays are using the MNI152NLin6Asym space of the Bianciardi atlas, since the CANLab2018 atlas doesn't use a single consistent space. Bianciardi is outlined in light blue, while CANLab2018 is outlined in white.

![Beliveau_5HTT](tracer_overlays/raphe_vs_dsab_beliveau_5HTT.png)
A high resolution map of DSAB ligand binding on 5-HT transporter shows good overlap with Bianciardi's Raphe (blue), but the CANLab2018 shows some outliers (white). In particular a nucleus near the pons is in a 5-HTT deadzone. This atlas is masked in the subthalamic area though so it's hard to judge the accuracy of the CLi-RLi structure which extends into that masked region. For that we turn to the next tracer map.

![Salvi_5HTT](tracer_overlays/raphe_vs_dsab_salvi_5HTT.png)
As you can see, this DSAB ligand binding map is truncated in the caudal arts of the image, but not in the subthalamic area where it shows good overlap with the CLi-Rli structure of Bianciardi (blue). We can corroborate these results by looking at binding for 5-HT1a autoreceptor, which will be less specific but should also be collocalized with the raphe nuclei

![Beliveau_5HT1a](tracer_overlays/raphe_vs_cumi_beliveau_5HT1a.png)
CUMI ligand binding map shows location of 5-HT1a sites that collocalize with the more dorsal Bianciardi atlas (blue) better than the more ventral CANLab2018 structures (white).

These results give me confidence that the approach taken by Bianciardi's group in their atlas construction works fairly well, although you should still find ways of corroborating any nuclear location with additional sources if it's critical to your question.

### Methods

The mapping from the MNI152NLin6Asym space to MNI152NLin2009cAsym space was performed using transformations computed by fmriprep 20.2.3 (LTS). Specifically fmriprep was run on MNI152NLin6Asym using MNI152NLin2009cAsym as the template.
 

Because we cannot distribute the original parcels nor any derivatives based on those parcels, the transformed versions
must be regenerated for each user on their own machine based on the files they themselves download. This entire process is automated by the bianciardi_create_atlas_obj.m script, which is invoked whenever this atlas is requested but not found by dependent scripts. The catch is that the transformation must be performed within matlab, since we cannot assume anything else about the user's environment. For instance, although fmriprep uses ANTs for registration, we cannot assume users have installed ANTs. This required converting the transformations computed by fmriprep into an SPM compatable format, since SPM is the only cross platform library for applying the transformation, and we can assume users have it installed because it's a dependency of this repo (by way of canlabCore). For documentation on the conversion process please refer to Neuroimaging_Pattern_Masks/templates/transforms/code/


The atlas is probablistic, but the probability labels aren't perfect. Six different studies produced the labels, and while we might expect probability labels to sum to 1 within study, they're less likely to do so across studies where one study's structures abut up against another study's structures. Unfortunately we don't even have probabilities summing to 1 within study. In particular DR and PAG (Bianciardi et al, 2015) and L_VTA_PBP and mRta (Singh et al, 2021) show slight overlaps (61 voxels and 1 voxel, respectively). Given how sparse these exceptions are and the fact that 4 of the 6 studies do have probabilities that sum to 1 I'm assuming these are just errors. I split the difference by renormalizing the probabilities so that they do sum to 1 both in these unusual cases and across studies, which results in a slight adjustment of probabilities relative to the raw data.


On the subject of probability labels, it's worth mentioning that something also appears to be wrong with the subthalamic nuclear probability maps. STh1 and STh2 seem to be intermixed in a very unusual way. These are supposed to be neighboring elipsoids, but are instead interleaved in a discontinuous manner. We have subthalamic nuclei in the CTI168 atlas too and those may be preferable to these.

## Comparison between spaces

Given the small scale of these structures accurate alignment may be more important for localization than for other atlases. The difference between MNI152NLin6Asym and MNI152NLin2009cAsym is minor but better safe than sorry. Extra effort is warranted to ensure proper alignment. In light of this we've used a subcortically weighted nonlinear alignment, instead of a full brain alignment, which only evaluates alignment cost in the thalamus, basal ganglia, hippocampal formation, brainstem and crebellum, without optimizing for cortical alignment. Additionally we've included 2mm versions of the atlas which were created by enchaining downsampling with any spantial transformation to minimize the number of interpolation steps.

### Copyright

Brainstem Navigator License

The following terms apply to all files associated with the Brainstem Navigator unless explicitly disclaimed in individual files.

1. Permission is granted to use the Brainstem Navigator without charge for non-commercial research purposes only. You may make verbatim copies of the Brainstem Navigator for personal use, or for use within your organization, provided that you include copies of all of the original copyright notices and associated disclaimers with each copy of the atlas.

2. YOU MAY NOT DISTRIBUTE COPIES of the Brainstem Navigator atlas labels and templates, or copies of files derived from them, to others outside your organization.

3. The Brainstem Navigator is for research purposes only and has not been approved for clinical use.

4. If you publish results whose generation used this atlas, you must provide attribution to the authors of the software by referencing the appropriate papers (see Reference List in Documentation).

5. IN NO EVENT SHALL THE MASSACHUSETTS GENERAL HOSPITAL, THE AUTHORS, OR THE DISTRIBUTORS BE LIABLE TO ANY PARTY FOR DIRECT, INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES, INCLUDING LOST PROFITS, ARISING OUT OF THE USE OF THIS SOFTWARE, ITS DOCUMENTATION, OR ANY DERIVATIVES THEREOF, EVEN IF THE AUTHORS HAVE BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

6. THE MASSACHUSETTS GENERAL HOSPITAL, THE AUTHORS, AND THE DISTRIBUTORS SPECIFICALLY DISCLAIM ANY WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT. THE BRAINSTEM NAVIGATOR IS PROVIDED ON AS IS BASIS, AND THE AUTHORS AND DISTRIBUTORS HAVE NO OBLIGATION TO PROVIDE MAINTENANCE, SUPPORT, UPDATES, ENHANCEMENTS, OR MODIFICATIONS.


### References

Hansen J, Shafiei G, Markello R, et al. Mapping neurotransmitter systems to the structural and functional organization of the human neocortex. Nature Neuroscience 25(11) 1569-1581, 2022. doi: 10.1038/s41593-022-01186-3

García-Gomar MG, Videnovic A, Singh K, Stauder M, Lewis LD, Wald LL, Rosen BR, Bianciardi M. Disruption of brainstem structural connectivity in RBD using 7 Tesla MRI. Mov Disord. 2021 Dec 29. doi: 10.1002/mds.28895. Online ahead of print. PMID: 34964520
For nuclei: iMRt, LC, LDTg_CGPn, PCRtA, PnO-PnC, ROb, RPa, sMRt, SubC

Singh K, García-Gomar MG, Bianciardi M. Probabilistic Atlas of the Mesencephalic Reticular Formation, Isthmic Reticular Formation, Microcellular Tegmental Nucleus, Ventral Tegmental Area Nucleus Complex, and Caudal-Rostral Linear Raphe Nucleus Complex in Living Humans from 7 Tesla Magnetic Resonance Imaging. Brain Connect. 2021 Oct;11(8):613-623. doi: 10.1089/brain.2020.0975. Epub 2021 Jun 17. PMID: 33926237.
For nuclei: CLi-RLi, isRt, mRt, MiTg-PBG, VTA-PBP

Singh K, Indovina I, Augustinack JC, Nestor K, García-Gomar MG, Staab JP, Bianciardi M. Probabilistic Template of the Lateral Parabrachial Nucleus, Medial Parabrachial Nucleus, Vestibular Nuclei Complex, and Medullary Viscero-Sensory-Motor Nuclei Complex in Living Humans From 7 Tesla MRI. Front Neurosci. 2020 Jan 23;13:1425. doi: 10.3389/fnins.2019.01425. PMID: 32038134; PMCID: PMC6989551.
For nuclei: LPB, MPB, Ve, VSM

García-Gomar MG, Strong C, Toschi N, Singh K, Rosen BR, Wald LL, Bianciardi M. In vivo Probabilistic Structural Atlas of the Inferior and Superior Colliculi, Medial and Lateral Geniculate Nuclei and Superior Olivary Complex in Humans Based on 7 Tesla MRI. Front Neurosci. 2019 Aug 7;13:764. doi: 10.3389/fnins.2019.00764. PMID: 31440122; PMCID: PMC6694208.
For nuclei: IC, SC, SOC (brainstem), and LG, MG (diencephalon)

Bianciardi M, Strong C, Toschi N, Edlow BL, Fischl B, Brown EN, Rosen BR, Wald LL. A probabilistic template of human mesopontine tegmental nuclei from in vivo 7T MRI. Neuroimage. 2018 Apr 15;170:222-230. doi: 10.1016/j.neuroimage.2017.04.070. Epub 2017 May 3. PMID: 28476663; PMCID: PMC5670016.
For nuclei: CnF, PTg, PMnR

Bianciardi m, Toschi N, Eichner C, Polimeni J, Setsompop K, Brown E, Hamalainen M, Rosen B, Wald L. In vivo functional connectome of human brainstem nuclei of the ascending arousal, autonomic and motor systems by high spatial resolution 7-Tesla fMRI. Magnetic Resonance Materials in Physics, Biology and Medicine 29(3) 451-462, 2016. doi: 10.1007/s10334-016-0546-3.

Bianciardi M, Toschi N, Edlow BL, Eichner C, Setsompop K, Polimeni JR, Brown EN, Kinney HC, Rosen BR, Wald LL. Toward an In Vivo Neuroimaging Template of Human Brainstem Nuclei of the Ascending Arousal, Autonomic, and Motor Systems. Brain Connect. 2015 Dec;5(10):597-607. doi: 10.1089/brain.2015.0347. Epub 2015 Aug 11. PMID: 26066023; PMCID: PMC4684653.
For nuclei: DR, ION, MnR, PAG, RMg, RN, SN (brainstem), and STh (diencephalon)

Garcia-Garcia A, Newman-Tancredi A, Leonardo E. P5-HT1A receptors in mood and anxiety: Recent insights into autoreceptor versus heteroreceptor functoin. Psychopharmacology 231(4), 623-636, 2014. DOI: 10.1007/s00213-013-3389-x

Son YD, Cho ZH, Kim HK, Choi EJ, L SY, C JG, Park CW, Kim YB. Glucose metabolism of the midline nuclei raphe in the brainstem observed by PET-MRI fusion. Neuroimage 59(2), 1094-1097, 2012. DOI: 10.1016/j.neuroimage.2011.09.036

Son Y, Cho Z, CHoi E, Kim J, Kim H, Lee S, Chi J, Park C, Kim J, Kim Y. Individually differentiated serotonergic raphe nuclei measured with brain PET/MR. Radiology 272(2), 541-548, 2014. DOI: 10.1148/radiol.14131547

###

Bogdan Petre
10/14/2023
