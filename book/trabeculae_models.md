# Trabecular flow modelling

## Table of contents

 1. [General notes](#general-notes)
 2. [Idealised trabecular geometries](#idealised-trabecular-geometries)
 3. [Preliminary results and recommendations for future work](#preliminary-results-and-recommendations-for-future-work)
 4. [Micro-CT geometries](#micro-ct-geometries)
 5. [Living heart model](#living-heart-model)
 6. [Contact details](#contact-details)

---

## Introduction

All files needed to replicate my results are in the [Trabecular flow sims](<https://imperiallondon.sharepoint.com/:f:/r/sites/CCIgroup/Shared%20Documents/Trabecular%20flow%20sims?csf=1&web=1&e=m2FKtw>) folder on SharePoint. All files, folders, paths I refer to in this document start from that folder.

## Idealised trabecular geometries

These have been created using 3 main softwares:

1. *Blender*: creates nice trabecular structures, but the resulting surface mesh contains errors and imprecisions, such as overlapping edges and duplicate faces. The amount of these errors prevents correction, and therefore use in CFD, which requires a precise definition of the fluid domain walls. Blender is Open Source.
2. *Mesh mixer*: while this software provides almost error free geometries, it can only create simple lattice structures that do not represent the trabeculae particularly well. Mesh mixer is Open Source.
3. *nTopology:* this software obtains intricate lattice structures that can represent the trabeculae well. It also allows you to control beam (trabecular) diameter and spacing (i.e., distance between beams). These parameters can also be controlled to change along a prescribed direction. Given its range of functionalities and its flexibility to adapt the generated structures to any input geometry, I'd recommend this software is used for further analyses. nTop in licenced.

### Lattice (trabeculae) structures in nTopology

Information on the software can be found [nTopology](<https://ntopology.com/>).

There are [resources](<https://ntopology.com/upcoming-and-resources/>), and especially under "Engineering design", you can find a useful video tutorial and webinars that are quite useful to learn how to operate *nTopology* and build lattice structures.

*Licence*: while the software is commercial, academics and researchers might be eligible for free licences (I have received and worked with one of these). More info at [nTopology](<https://ntopology.com/education/>).

Previously designed geometries with their design workflow can be found in:

`\Trabecular flow sims\Idealised Models\Geometries & Templates\nTopology`

For the idealised (square block) models, and in:

`\Trabecular flow sims\LV models\nTopology Geometries & Template`

For the left ventricular model.

I would recommend going through the tutorials before working on these files, are they require familiarity with *nTopology* project tree structure (which nests elements) and lattice structure generation.

Key component of the design workflow (see files) are

- Type of lattice structure: the structure that gives the best qualitative agreement with trabecular structure is the Voronoi lattice (this is also in line with previous literature works).
- definition of beam dimension and spacing. These vary linearly in the square block geometries ("Ramp thickness" and "Ramp spacing"). In the LV model, the thickness is constant, while the spacing requires some more work, as at the moment is a by-product of the seed point spacing used to generate the Voronoi structure.
- Surface mesh: this is the last step that needs to be taken before exporting. I also recommend performing smoothing before remeshing. This remeshing is necessary to ensure that the file is not excessively large (nTop has the tendency to generate extremely large files) and to ensure quality of the surface mesh. Geometries exported after this step are generally immediately compatible with FV, without any further correction. Should a correction be required I recommend using the mesh analysis tools within mesh mixer and/or 3D transvidia.

### Flow Vision Simulations

Information regarding the fluid solver – FlowVision – can be found [here](<https://flowvisioncfd.com/en/>). With documentation and tutorials located [here](<https://flowvisioncfd.com/en/support-page-en/blog-en>). I highly recommend reading the documentation and executing the tutorials before starting to work on the shared setup files.

Once you are familiar with FV setup, it should be fairly straightforward to replicate previous simulations, for which all setup files can be found here:

`\Trabecular flow sims\Idealised Models\FV Simulations Setup and Results\FV_ClientDirectory`

`\Trabecular flow sims\LV models\FV Simulations Setup and Results\FV_ClientDirectory`

Corresponding results are in:

`\Trabecular flow sims\Idealised Models\FV Simulations Setup and Results\FV_ServerDirectory`

`\Trabecular flow sims\LV models\FV Simulations Setup and Results\FV_ServerDirectory`

**Licence**. FV is a licenced software. We have a total of 40 cores shared with the Xu Group. Of these, 16 are used by Zhongjie Yin ([z.yin20@imperial.ac.uk](<mailto:z.yin20@imperial.ac.uk>)) while 24 are dedicated to this project. The current licence expires on Jan 13th 2023. The licence server is managed by ICT. Please get in touch with me ([s.pirola@imperial.ac.uk](<mailto:s.pirola@imperial.ac.uk>)) or Zhongjie ([z.yin20@imperial.ac.uk](<mailto:z.yin20@imperial.ac.uk>)) should additional access to the licence server need to be arranged.

The software is pre-installed (including licence set-up and setup for FSI – 1 or 2-way coupling with Abaqus) on workstation ce-spirola-dt (Windows), which can be used for this work. The workstation is located in ACEX 1M17B, Chemical Engineering Department, South Kensington Campus. All files uploaded on SharePoint are also already available on this workstation. Please contact [s.pirola@imperial.ac.uk](<mailto:s.pirola@imperial.ac.uk>) (machine owner) to request remote access, (if applicable and needed) admin rights, and for info on how to navigate and use the workstation.

### Notes on the simulation setup

1. Simulations conducted with *nTopology* geometries use a non-Newtonian (Carreau-Yasuda, see code below) blood flow model. This is defined in `Substances\?Blood\viscosity` in the FV setup tree. Here the shear rate is defined as the room mean square of the S-criterion (readily available as a FV variable). This has been confirmed with FV developers. However, I still recommend running a couple of simulations with a known geometry (e.g. backward facing step) to further verify this and the correct implementation of the non-Newtonian model in FV.

**Code for the Carreau-Yasuda model:**

$\mu_{min}$=0.0035;

$\mu_{max}$=0.16;

$\lambda$=8.2;

$\alpha$=0.64;

$n$=0.2128;

$$
\mu_{min} + (\mu_{max} - \mu_{min}) \times (1+(\lambda \sqrt{\dot{\gamma}})^a)^\frac{n-1}{a}
$$

2. Use of trabeculae geometries: trabeculae are designed separately from the fluid volume surface and imported as moving bodies. This is so that a modular setup could be obtained in which trabecular surfaces are easily replaced and an airtight fluid volume is always ensured. Also, this allows for the trabecular surfaces to be treated as separate entities, for which a specific drag force can be calculated. This will also facilitate implementation in Abaqus for FSI, though the overlap between the trabecular and left ventricular surfaces will need to be corrected.

## Preliminary results and recommendations for future work

### Idealised Trabecular Geometries

Figures showing the computational domain and summarising key preliminary results are shown in the ppt file [SPirola\_summary.ppt](<https://imperiallondon.sharepoint.com/:p:/r/sites/CCIgroup/Shared%20Documents/Trabecular%20flow%20sims/SPirola_Summary.pptx?d=wc8b2f4ca9aee49fca2641c7a0a542b52&csf=1&web=1&e=ih2eR3>) Overall, preliminary results show that increasing the beam dimension and the trabecular compaction causes an increase in drag force. Please note: the fluid domain has been designed to replicate half of the mitral valve opening (indeed a symmetry boundary condition is used at the top wall). Reported results have been simulated with 0.1 m/s inlet velocity. I'd recommend testing the drag force trend under different physiological Re numbers.

Currently only drag force is calculated. A more robust approach would be to calculate the drag coefficient. To do so, a robust approach for calculation of projected frontal area will need to be developed (please have a look at the theory behind conversion of drag force into drag coefficient).

### LV Trabecular Geometries

Results available in `Trabecular flow sims\LV models\FV Simulations Setup and Results\FV_ServerDirectory` serve as a mere proof of feasibility showing designed geometries are compatible with simulations. Indeed, all simulations run so far with this geometry aimed at testing the model geometry. Further work on simulation setup is needed to extract interesting results. Firstly, at the mitral inlet a steady velocity of 0.5 m/s was imposed, while 0 pressure was used at the aortic outlet. A flow waveform typical of a healthy mitral valve should be used at the inlet during diastole, while an aortic waveform should be used at the aortic outlet during systole. This will need to be done while making sure the mass balance is satisfied: I suggest looking into hybrid solutions for boundary conditions allowing to switch between Dirichlet and Newman (i.e., flow and pressure).

This work should ultimately aim to employ a moving wall approach. All preliminary work done in this direction is summarised in folder "Moving walls in FV". I suggest watching the video first, then looking at the other files. Please keep the video confidential.

## Micro-CT geometries

We received/collected micro-CT imaging data from various sources:

- *Great Ormond Street hospital (GOSH)*: data and preliminary simulations (folder "FlowVision") can be found in `\Trabecular flow sims\Others\GOSH`. These data were shared as STL files (we do not have the raw images). Shared files are in `\Trabecular flow sims\Others\GOSH\Shared`

Other files in the folder are derived from postprocessing of these original files to remove errors in the surface mesh. "3M" stands for 3-matic, "3DTV" 3D Transvidia: these are postprocessing software used to correct the geometries. The CAD files (.SLDPRT) contain the fluid volume domain.

- *University of Birmingham*: data and preliminary simulations (folder "FlowVision") can be found in `Trabecular flow sims\Others\Birmingham`. The folder "Dicom" contains the raw images, while the folder "DicomRadiant" contains postprocessed images. ".mcs" files are Mimics files containing segmentations of various regions contained in the images. Unfortunately, due to the dimensions of these microCT data, processing the full raw images has proven to be prohibitive, therefore raw images have been cropped to reduce the processing burden, so different files contain different cropped regions.

While we still have these data and corresponding simulation results (preliminary) the necessity to crop the domain for processing and the lack of contrast within the heart chambers has forced us to reconstruct square/rectangular samples of trabeculae. While these have been and are extremely useful to design the synthetic trabeculae (see above), simulations conducted with these geometries are unrealistic and have therefore been paused for the time being.

Collecting new micro-CT data. Various department in Imperial College London have a micro-CT scan that can potentially be used to collect new CT data with better contrast. One such scan is in the Chem.Eng Department: for more information, please contact Edward (Ed) Bailey ([edward.bailey@imperial.ac.uk](<mailto:edward.bailey@imperial.ac.uk>)), Experimental Officer. A new scan is being installed in ICTEM: Michela Noseda ([m.noseda@imperial.ac.uk](<mailto:m.noseda@imperial.ac.uk>)) and Lukas Mach ([l.mach@imperial.ac.uk](<mailto:l.mach@imperial.ac.uk>) ) should be able to provide more information on this. Also, we have a sample ready to be scanned (contrast should be added): reach out to Richard Jones ([R.Jones@rbht.nhs.uk)](<mailto:R.Jones@rbht.nhs.uk>) and Lukas Mach for this.

## Living heart model

Access to the Living heart model (LHM) is subject to successful completion of the LHM agreement by Imperial, which is still pending. After the agreement is signed, we'll have access to the LHM that can be used to run FEA simulations in Abaqus. The model does not contain trabeculae as yet, but an initial feasibility assessment with the LHM Team concluded that, with training on the 3DEXPERIENCE Platform and its LHM MVA Evaluation Model, trabeculae could be added and simulated.

To be granted access to this, provided the LHM agreement is in place, please get in touch with [clint.davies-taylor@3ds.com](<mailto:clint.davies-taylor@3ds.com>) who acts as a first point of contact between Simulia and Imperial.

Should you be interested in FEA using the LHM, I'd recommend contacting them sooner rather than later, as our initial assessment was of about 6 weeks of full-time training only to master the 3DEXPERIENCE Platform.

## Contact details

I am fully available should further details and clarification be needed. You can reach me using my Imperial email [s.pirola@imperial.ac.uk](<mailto:s.pirola@imperial.ac.uk>) , and/or my TU Delft email [s.p.drpirola@tudelft.nl](<mailto:s.p.drpirola@tudelft.nl>) which I will probably monitor more frequently as time goes by.
