# OTTR
OTTR - The Oral ToxiciTy Reporter

<p>OTTR is a mechanistic QSAR model to predict mammalian acute oral toxicity. Details on the development and evaluation of the model are available in the accompanying QMRF and associated publication: <a href="https://academic.oup.com/toxsci/advance-article-abstract/doi/10.1093/toxsci/kfad025/7083447">Wijeyesakere, S.J., Auernhammer, T., Parks, A., and Wilson, D. (2023). Profiling mechanisms that drive acute oral toxicity in mammals and its prediction via machine learning. Published in Toxicological Sciences.</a></p>

<h2>How to install OTTR</h2>

1.	Make sure you have KNIME installed, together with all cheminformatics and data analytics nodes
2.	Download the KNIME workflow for OTTR (OTTR-XX.XX.XX.knwf)
3.	Download the ZIP archive containing the machine-learning models (submechanistic_ld50_models.zip)
4.	Unzip the ZIP archive machine-learning model and place the unzipped folder “submechanistic_ld50_models” in a location that is readily accessible to you and other users on your system
1.	On a Mac or Linux/UNIX system, we recommend placing this folder in “/usr/local/bin”
2.	On a Windows system, we recommend placing this folder in a directory in root of your C-drive (e.g., in C:\OTTR)
5.	Double-click the KNIME workflow for OTTR. Once it has been loaded into KNIME, simply click the “Save as…” button on the upper right-hand side of the workflow to save it to your KNIME profile
6.	Double-click the blue “Run OTTR” node and change the location in the text box to point to the location of the folder containing the machine-learning models (from step 4)
7.	You are now ready to run OTTR!

Questions? Please contact us via e-mail at: OTTR@dow.com

