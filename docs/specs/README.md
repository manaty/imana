# Functional Specifications

Glossary
--------
Medical Organization (MedOrg): a Doctor office, a clinic, an hospital,... any medical organisation that could use the Imana platform.

Dataprocessing Organization (DataOrg) : Any IT company, University laboratory, individual... that will build then provide an Image Analysis Service (ImanaService) to the Imana platform.

Roles
-----
* MedAdmin : Administrator of a MedOrg 
* Practitioner : Any healthcare professional that belong to an organization and can upload ultrasound images in the platform.
* DataAdmin : Administrator of a DataOrg that will use the images of the plateform to provide analyzis service (typically through usage of AI models)

A user could be both MedAdmin and Practitioner, he cannot be DataAdmin and something else.

Data Model (simplified)
-----------------------
![Imana Data Model](https://raw.githubusercontent.com/manaty/imana/master/docs/specs/Imana%20data%20model.png)

Processes
---------

### MedAdmin and DataAdmin creation
When an medical or data organization member contacts us, for instance after visiting https://manaty.org
we creates him a user on meveo & keycloack with a temorary password and with the email he used to send him notifications

all operations for administrator management are handeled by the meveo admin.

### Practitioner creation,update, deletion
The MedAdmin can create and manage practitioner accounts.

### ImanaService creation
When a DataOrg provides a Image Analysis Service, Manaty validates that the service comply to the ImanaService technical interface then configure it in Meveo. The meveo admin creates then an ImanaService entity that will be available to all MedOrgs.
An ImanaService have 2 flags :

* isTraining : determines if the service is currently accepting measures from MedOrgs to train itself

* isAnalysing : determines if the service is currently ready to analyze measures

Once the ImanaServices are created then can be managed by the MedAdmins of its MedOrg.

### MedOrg configuration
A medical organization have the choice among the Image analysis services it want to train and to use. Any MedAdmin can select among all the available ImanaServices the ones it allows its organization to train and use.

An organization could also choose its measures to be public. In that case all services can use its data and it can use all services.

### Visit creation
When a practitioner perform an ultrasound scann session with a patient, it uses the app to create a Visit.

He then fill the minimal information needed to train or use the image analysis services while not revealing any information about the patient that could reveal its identity.
Then for each ultrasound picture he creates a measure indicating the position of the probe (Upper Left lobe, ...) and he uploads the associated picture.

In case he want the measure to be used as a training set for analysis services, he adds to it an analysis result.

In case he needs an automated analysis of the patient condition, he submit the visit for automated analysis.

In case he needs a manual analysis of the patient condition, he submit the visit for manual analysis.

He can then wait for the results or asked to be notified when results are available.
 
### Analysis result creation
When a practioner connects to its account he can see all the visits made by others for which manual analysis is required.

He can then visualize or download the images and create Analysis results.

For Visit marked for automatic results, an API accesible to Image Analysis service providers allow them to list all the visits corresponding to organizations that agreed to submit their data for training and another rest API allow them to automatically create Result Analysis on these visits.

