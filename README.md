# Deep-CXR-Score
In this project, the model for creation of Deep-CXR-Score is described. 

\section{Study Population} 
\label{sec:methods:methods_one}
\begin{figure}
    \centering
    \includegraphics[width=0.95\linewidth]{figures/studyflowchart.drawio.png}
    \caption[Study flowchart of study]{\textbf{Study flowchart.} Please note that more than 95\% of patients/their parents or legal guardians referred to our center.}
    \label{fig:enter-label}
\end{figure}
\paragraph{Ethics}
This observational retrospective study (clinicaltrials.gov identifiers NCT00760071, NCT02270476) received approval from the institutional ethics committee of the University of Heidelberg (S-211/2011, S-370/2011). It included all pediatric and adult patients with \ac{CF} (mean age 9.2±6.1yr, range 0 – 35yr) who underwent clinically indicated chest \ac{MRI} and chest x-ray, or \ac{PFT} concurrently at our center between 2006 and 2023. Some patients were included in our previous reports; however, those studies did not include chest x-ray or deep learning analysis (\cite{8,9,11,13,16}).

\paragraph{Recruitment}
In total, 197 patients were included in the study (Figure 2.1). Please note that more than 95\% of patients/their parents or legal guardians referred to our center consented to participating in the study. 126 patients with \ac{CF} were identified by newborn screening (\cite{37}). The diagnosis of \ac{CF} was confirmed by increased sweat Cl-concentrations ($\geq 60$mmol/L), \ac{CFTR} mutation analysis. In pancreatic sufficient patients with borderline sweat test results (sweat Cl-30-60 mmol/L) assessment of \ac{CFTR} function is carried out in rectal biopsies as previously described (\cite{38}).Because contrast material was not administered routinely in infants as per protocol, and some parents refused its use in older participants, the MRI perfusion score was available for 923 \ac{MRI} examinations (88.6\%) (Table 1). 

\begin{table}[htbp]
\caption[Characteristics of patients and material]{\textbf{Characteristics of patients for deep learning and centroid classifier}}
\centering
\scriptsize            % remove or change if you need a different font size
\begin{adjustbox}{width=\textwidth}
\begin{tabular}{lcccccccc}
\toprule
\multirow{2}{*}{} &
\multicolumn{4}{c}{\textbf{Deep-learning study on chest x-ray}} &
\multicolumn{3}{c}{\textbf{Centroid classifier on PFT}} &
\multicolumn{1}{c}{\textbf{Combined}}\\
\cmidrule(lr){2-5}\cmidrule(lr){6-8}\cmidrule(lr){9-9}
 & Training & Validation & Test & P & Training & Test & P & Total\\
\midrule
Patients, n (\%)                 & 126 (79\%) & 16 (10\%) & 17 (11\%) & — & 177 (92\%) & 16 (8\%)  & — & 197\\
Sex (m / f)                       & 67 / 59    & 10 / 6    & 13 / 4    & — & 90 / 87    & 12 / 4    & — & 105 / 92\\
MRI examinations, n              & 501        & 77        & 66        & — & 892        & 95        & — & 1041\\
Average MRI contribution         & $4.0\pm2.3$& $4.8\pm2.1$& $3.9\pm2.7$& — & $5.0\pm3.4$& $5.9\pm3.3$& — & $5.3\pm3.7$\\
Age (yrs)                         & $5.4\pm6.1$& $5.6\pm5.9$& $7.2\pm9.1$& 0.19 & $7.8\pm8.8$& $7.1\pm8.4$& 0.08 & $7.5\pm8.9$\\
Range                             & 0.1–20.9   & 0.1–18.2   & 0.1–29.2   & — & 0.1–35.3   & 0.1–29.2   & — & 0.1–35.3\\
Height (cm)                       & $101.2\pm39.3$& $104.2\pm38.0$& $101.2\pm48.9$& 0.04 & $108.9\pm41.0$& $109.1\pm44.4$& 0.23 & $105.7\pm42.2$\\
Range                             & 49.5–183.2 & 57.8–164.0 & 53.0–182.0 & — & 48.5–195.0 & 53.0–179.0 & — & 48.5–195.0\\
Weight (kg)                       & $21.1\pm18.6$& $20.3\pm16.2$& $22.5\pm23.9$& 0.04 & $24.1\pm19.6$& $24.6\pm22.7$& 0.08 & $23.2\pm20.1$\\
Range                             & 3.11–73.3  & 4.0–53.1   & 3.3–77.0   & — & 2.7–80.0   & 3.3–77.0   & — & 2.7–80.0\\
BMI (kg/m$^{2}$)                  & $15.9\pm2.5$& $15.8\pm2.3$& $16.0\pm3.2$& 0.21 & $16.1\pm2.6$& $16.4\pm3.0$& 0.01 & $16.1\pm2.7$\\
Range                             & 11.1–27.5  & 12.0–20.2  & 11.8–24.0  & — & 11.1–27.5  & 11.8–24.0  & — & 11.1–27.5\\
chest x-ray images, n (\%)              & 501 (78\%) & 77 (12\%)  & 66 (10\%)  & — & — & — & — & 644\\
meanLCI, \textit{n} (\%)          & — & — & — & — & 697 (90\%) & 78 (10\%) & — & 775\\
meanLCI, mean                     & — & — & — & — & $9.5\pm3.7$ & $9.0\pm4.0$ & 0.80 & $9.5\pm3.7$\\
Range                             & — & — & — & — & 5.4–25.7 & 6.2–21.1 & — & 5.4–25.7\\
ppFEV$_1$, \textit{n} (\%)        & — & — & — & — & 654 (91\%) & 64 (9\%) & — & 718\\
ppFEV$_1$, mean (\%)              & — & — & — & — & $81.0\pm20.1$ & $83.9\pm19.4$ & 0.32 & $81.2\pm20.0$\\
Range                             & — & — & — & — & 23.5–137.1 & 53.4–116.7 & — & 23.5–137.1\\
\midrule
\multicolumn{9}{l}{\textbf{CFTR genotype class, n (\%)}}\\
F/F            & 56 (44.4\%) & 7 (43.8\%) & 9 (52.9\%) & — & 80 (45.2\%) & 8 (50.0\%) & — & 91 (46.2\%)\\
F/MF           & 43 (34.1\%) & 5 (31.3\%) & 1 (5.9\%)  & — & 54 (30.5\%) & 1 (6.3\%)  & — & 56 (28.4\%)\\
F/G            & 4  (3.2\%)  & 1 (6.3\%)  & 0          & — & 5  (2.8\%)  & 0          & — & 5  (2.5\%)\\
F/RF           & 6  (4.8\%)  & 0          & 1 (5.9\%)  & — & 12 (6.8\%)  & 1 (6.3\%)  & — & 13 (6.0\%)\\
F/other        & 2  (1.6\%)  & 0          & 2 (11.8\%) & — & 6  (3.4\%)  & 2 (12.5\%) & — & 8  (4.0\%)\\
MF/MF          & 9  (7.1\%)  & 2 (12.5\%) & 1 (5.9\%)  & — & 12 (6.8\%)  & 1 (6.3\%)  & — & 13 (6.6\%)\\
MF/RF          & 2  (1.6\%)  & 1 (6.3\%)  & 1 (5.9\%)  & — & 3  (1.7\%)  & 1 (6.3\%)  & — & 4  (2.0\%)\\
MF/G           & 3  (2.4\%)  & 0          & 1 (5.9\%)  & — & 3  (1.7\%)  & 1 (6.3\%)  & — & 4  (2.0\%)\\
MF/other       & 0           & 0          & 0          & — & 0           & 0          & — & 0\\
RF/RF          & 0           & 0          & 0          & — & 1  (0.6\%)  & 0          & — & 1  (0.5\%)\\
Other/other    & 1  (0.8\%)  & 0          & 1 (5.9\%)  & — & 1  (0.6\%)  & 1 (6.3\%)  & — & 2  (1.0\%)\\
\bottomrule
\end{tabular}
\end{adjustbox}
\caption*{\footnotesize%
BMI = body-mass index; CFTR = cystic-fibrosis transmembrane-conductance regulator;  
F = loss-of-function mutation; MF = minimal-function mutation; G = gating mutation; RF = residual-function mutation;  
P = P-value.  
Values are provide in \textit{n} (\%), mean $\pm$ SD, or range.  
Percentages for Patients, chest x-ray images, LCI (N$_2$), and ppFEV$_1$ refer to the total number in each experiment;  
percentages for \ac{CFTR} genotype classes refer to their subgroup.  
Some patients did not undergo 4-D perfusion \ac{MRI}, which is required for determination of the \ac{MRI} perfusion and global scores.
Comparison of two groups were performed by Mann-Whitney U-test, and multiple group comparison were performed using ANOVA on ranks.}
\end{table}


\begin{table}[htbp]
\caption[\ac{MRI} scores distribution]{\textbf{Distribution of \ac{MRI} scores across various data groups for deep learning and centroid classification.}}
\centering
\scriptsize            % shrink to fit – adjust or remove as needed
\begin{adjustbox}{width=\textwidth}
\begin{tabular}{lcccccccc}
\toprule
\multirow{2}{*}{} &
\multicolumn{4}{c}{\textbf{Deep-learning study on chest x-ray}} &
\multicolumn{3}{c}{\textbf{Centroid classifier on PFT}} &
\multicolumn{1}{c}{\textbf{Combined}}\\
\cmidrule(lr){2-5}\cmidrule(lr){6-8}\cmidrule(lr){9-9}
 & Training & Validation & Test & P & Training & Test & P & Total\\
\midrule
\textbf{MRI global score}          & $12.6\pm7.8$ & $13.7\pm8.8$ & $14.1\pm9.9$ & 0.57 & $13.7\pm8.3$ & $13.7\pm9.3$ & 0.99 & $13.7\pm8.4$\\
Range                              & 0–41 & 0–42 & 0–42 & — & 0–49 & 0–44 & — & 0–49\\
Prevalence, n (\%)                 & 501 (97.8) & 77 (96.1) & 66 (95.5) & — & 1061 (98.4) & 115 (97.4) & — & 1176 (98.3)\\
\addlinespace
\textbf{MRI morphological score}      & $8.7\pm5.4$ & $9.5\pm6.2$ & $9.7\pm7.3$ & 0.62 & $9.7\pm5.8$ & $9.8\pm7.0$ & 0.84 & $9.7\pm5.9$\\
Range                              & 0–30 & 0–30 & 0–32 & — & 0–39 & 0–33 & — & 0–39\\
Prevalence, n (\%)                 & 501 (97.4) & 77 (96.1) & 66 (95.5) & — & 1061 (98.0) & 115 (97.4) & — & 1176 (98.0)\\
\addlinespace
\textbf{MRI perfusion score}       & $4.3\pm2.9$ & $4.7\pm3.3$ & $5.1\pm3.1$ & 0.62 & $4.5\pm3.0$ & $4.6\pm3.1$ & 0.68 & $4.5\pm3.0$\\
Range                              & 0–12 & 0–12 & 0–12 & — & 0–12 & 0–12 & — & 0–12\\
Prevalence, n (\%)                 & 449 (88.6) & 69 (91.3) & 57 (93.0) & — & 939 (90.1) & 96 (92.7) & — & 1035 (90.3)\\
\addlinespace
\textbf{Bronchiectasis / wall thickening} & $5.3\pm2.2$ & $5.6\pm2.3$ & $5.8\pm2.7$ & 0.52 & $5.7\pm2.1$ & $5.8\pm2.4$ & 0.64 & $5.7\pm2.2$\\
Range                              & 0–12 & 0–12 & 0–12 & — & 0–12 & 0–12 & — & 0–12\\
Prevalence, n (\%)                 & 499 (97.8) & 75 (98.7) & 64 (96.9) & — & 1055 (98.6) & 113 (98.2) & — & 1168 (98.5)\\
\addlinespace
\textbf{Mucus plugging}            & $2.4\pm2.4$ & $2.5\pm2.7$ & $2.9\pm3.0$ & 0.88 & $2.7\pm2.5$ & $2.8\pm2.6$ & 0.72 & $2.7\pm2.5$\\
Range                              & 0–12 & 0–12 & 0–11 & — & 0–12 & 0–11 & — & 0–12\\
Prevalence, n (\%)                 & 499 (72.3) & 75 (68.0) & 64 (78.1) & — & 1055 (76.3) & 113 (84.1) & — & 1168 (77.1)\\
\addlinespace
\textbf{Pleural findings}          & $0.6\pm1.0$ & $0.8\pm1.1$ & $0.9\pm1.3$ & 0.86 & $0.8\pm1.1$ & $0.8\pm1.4$ & 0.64 & $0.8\pm1.2$\\
Range                              & 0–5 & 0–4 & 0–5 & — & 0–7 & 0–6 & — & 0–7\\
Prevalence, n (\%)                 & 497 (39.4) & 75 (44.0) & 63 (42.9) & — & 1050 (42.7) & 112 (37.5) & — & 1162 (42.2)\\
\addlinespace
\textbf{Abscess / sacculation}     & $0.1\pm0.4$ & $0.1\pm0.4$ & $0.0\pm0.3$ & 0.47 & $0.1\pm0.6$ & $0.1\pm0.5$ & 0.58 & $0.1\pm0.5$\\
Range                              & 0–3 & 0–3 & 0–2 & — & 0–6 & 0–3 & — & 0–6\\
Prevalence, n (\%)                 & 496 (5.4)  & 75 (4.0)  & 63 (3.2)  & — & 1050 (7.1) & 112 (4.4)  & — & 1164 (6.9)\\
\addlinespace
\textbf{Consolidations}            & $0.4\pm0.8$ & $0.7\pm1.1$ & $0.5\pm1.1$ & 0.01 & $0.5\pm1.0$ & $0.5\pm1.2$ & 0.62 & $0.5\pm1.0$\\
Range                              & 0–5 & 0–5 & 0–4 & — & 0–8 & 0–5 & — & 0–8\\
Prevalence, n (\%)                 & 497 (25.1) & 75 (42.7) & 63 (20.6) & — & 1053 (27.8) & 112 (22.3) & — & 1165 (27.3)\\
\bottomrule
\end{tabular}
\end{adjustbox}
\caption*{\footnotesize%
P = P-value.  
Values are provide with mean $\pm$ SD, range, or \textit{n} (\%).  
Prevalence percentages refer to the total number of \ac{MRI} examinations in each experiment.  
Some patients lacked 4-D perfusion \ac{MRI}, which is required for determination of the \ac{MRI} perfusion and global scores.
Comparison of two groups were performed by Mann-Whitney U-test, and multiple group comparison were performed using ANOVA on ranks.}

\end{table}
The population was split into two subgroups for separate analyses. Subgroup 1 uses chest x-ray paired with the chest \ac{MRI} score, and the other using \ac{PFT} paired with the chest \ac{MRI} score for \ac{AI} training. A test set with both chest x-ray and \ac{PFT} was used to test the predicted \ac{MRI} score. The first subgroup comprised 644 chest x-rays with corresponding same-day (mean -0.2 ± 0.7 d, range 0 - 2 d) chest \ac{MRI} examinations from 159 \ac{CF} patients. These patients were randomly divided into training, validation, and test groups in an 80\%-10\%-10\% ratio, ensuring balanced representation across the number of chest x-ray images and the distribution of age, global disease severity, as well as the specific disease severity by control of the corresponded visual \ac{MRI} global score or specific visual \ac{MRI} scores (Table 1).

The second subgroup centered on \ac{PFT} included 987 \ac{MRI} scans with corresponding same-day \ac{PFT} (\ac{LCI}: 0.1 ± 0.2 d, range 0 - 3 d; \ac{FEV1}: 0.1 ± 0.1 d, range 0 - 2 d) from 193 patients. This subgroup was divided into training and test groups with a 90\%-10\% ratio due to the \ac{AI} approach. Notably, all patients in the test group from the first chest x-ray subgroup were also included in the test group of the second subgroup, which means that the \ac{PFT} subgroup’s test set is based on the chest x-ray subgroup’s split. (Table 1).

\begin{figure}
    \centering
    \includegraphics[width=.8\linewidth]{figures/xraysequense.drawio.png}
    \caption[The chest x-ray sequence for therapy response test.]{\textbf{The chest x-ray sequence for therapy response test.}}
    \label{fig:enter-label}
\end{figure}

To further validate our deep learning chest x-ray–predicted \ac{MRI} score system, we analyzed another cohort of 20 adults with \ac{CF}, who never took part into the training process (mean age 29.8 ± 5.9 years; range 19.8–43.2 years). Each patient had a frontal chest x-ray taken an average of 12.2 ± 10.5 months (range 0.1–45.9 months) before starting \ac{ETI} therapy. A follow-up chest x-ray (months from X-RAY1: 31.6 ± 10.9 months, 12.1-52.4 months) was obtained after the patients had been on \ac{ETI} for at least one month on average after \ac{ETI} initiation. The mean duration of \ac{ETI} therapy at the time of this scan was 19.4 ± 10.2 months (range 0.8–37.1 months). 15 of the 20 patients also had an earlier x-ray, acquired a mean of 21.8 ± 14.0 months (range 2.5–51.4 months) before X-RAY2, when they were not yet receiving \ac{ETI}. This longitudinal set of up to three chest x-rays per patient allowed us to evaluate the score’s response before \ac{ETI} and its responsiveness after therapy began (Figure 2.2).

\section{Therapy Regimes}
\label{sec:method:methods_two}
All patients were followed at the \ac{CF} center at the University Hospital Heidelberg and standard of care was provided according to the European Best Practice Guidelines including physiotherapy for airway clearance, inhalation with either isotonic or hypertonic saline starting at diagnosis, and substitution of pancreatic enzymes and fat-soluble vitamins in patients with pancreatic insufficiency. In case of worsening of respiratory symptoms or increasing \ac{MRI} scores, inhaled dornase alfa was added. Detection of Pseudomonas aeruginosa led to an antibiotic treatment aiming at eradication. All other pathogens were generally treated with oral antibiotics if respiratory symptoms were present, however, no antibiotic prophylaxis was given (\cite{39,40}). 

In patients who took part in the \ac{AI} training, validation and testing, 4 patients received ivacaftor therapy since 2013, affecting 11 \ac{MRI} examinations, and 23 patients aged six years and older homozygous or heterozygous for the F508del \ac{CFTR} mutation received ivacaftor/tezacaftor, affecting 32 examinations, respectively. 23 patients aged six years and older homozygous  for the F508del \ac{CFTR} mutation received lumacaftor/ivacaftor since its approval end of 2015 in Europe, and additional 29 patients aged two years and older since 2019, affecting 98 \ac{MRI} examinations in total. Of note, \ac{MRI} from 5 patients receiving highly effective triple \ac{CFTR} modulator therapy were excluded from the study, resulting in the exclusion of 6 \ac{MRI} examinations.

In the patient group to validate the model's therapy response, all of those 20 patients were treated with standard therapy before X-RAY1, and then accepted \ac{ETI} therapy in addition between X-RAY2 and X-RAY3.

\section{Pulmonary Function Tests} 
\label{sec:methods:methods_five}
MBW was conducted on the same day as \ac{MRI} using the \textit{Exhalyzer D} system (Eco Medics, Dürnten, Switzerland), employing 100\% oxygen to flush out resident nitrogen (N\textsubscript{2}) from the lungs via a mouthpiece interface [(\cite{21}). All measurements were analyzed with \textit{Spiroware 3.3.1} software to compute \ac{LCI} (Eco Medics) (\cite{22,48}). In case children < 5 years were sedated with chloral hydrate, \ac{MRI} and \ac{MBW} were performed in the same session.

Full-body plethysmography and/or spirometry (\textit{MasterScreen Body}, E. Jaeger) were performed on the same day as \ac{MRI}, adhering to the standards set by the European Respiratory Society and the American Thoracic Society, \ac{ppFEV1} were computed accordingly (\cite{49,50}).

A dataset-based z-score normalization was applied on LCI and \ac{ppFEV1} before the \ac{AI}-based analyses.

\section{Image Acquisition and Assessment} 
\label{sec:methods:methods_three}
\subsection{MRI}
\begin{figure}
    \centering
    \includegraphics[width=1\linewidth]{mriscoresystem.png}
    \caption[The chest \ac{MRI} scoring system]{\textbf{The morphological and functional chest \ac{MRI} scoring system for \ac{CF}.}}
    \label{fig:enter-label}
\end{figure}
Standardized \ac{MRI} of the chest was performed after first diagnosis or referral to our center starting at the age of three months and then repeated annually using three similar 1.5 T scanner models from the same manufacturer (Magnetom Symphony, Magnetom Avanto, and Magnetom Aera, Siemens Healthineers, Erlangen, Germany), with the protocol kept constant over the whole study period apart from minor adaptations to new software versions as previously described (\cite{31,20,28,29,42,43,44,23,32,46,47}). In brief, T1-weighted sequences before and after intravenous application of contrast material, and T2-weighted sequences before contrast were acquired. Four-dimensional lung perfusion imaging was performed with intravenous injection of macrocyclic Gadolinium-based contrast material (0.1 mmol/kg body weight of gadobutrol [Gadovist; Bayer Schering AG, Germany] or gadoteric acid [Dotarem, Guerbet, France]) at a rate of 3–5 ml/s. Children $\leq 5$ years were routinely sedated with oral or rectal chloral hydrate (100 mg/kg body weight, maximum dose of 2 g) and monitored during the \ac{MRI} examination by MR-compatible pulse oximetry. Renal function impairment was controlled for before contrast application but did not preclude any patient from undergoing contrast-enhanced \ac{MRI}. Contrast material application required for 4D perfusion imaging was avoided per protocol in infancy due to prescription regulations.

All \ac{MRI} examinations were assessed by the same validated reader (MOW) who also evaluated all previous studies using the established chest \ac{MRI} scoring system (\cite{31,20,28,29,42,43,44,23,32,46,47,25}). Of note, the results from previous \ac{MRI} examinations were available to the reader for direct comparison. The \ac{MRI} score assigns a numerical disease severity score to each of the five lobes and the lingula as 0 = no presence, 1 = <50\% of a lobe affected, and 2 = $\geq 50\%$ of a lobe affected. This scoring applies to each morphological feature: bronchiectasis/wall thickening, mucus plugging, sacculation/abscess, consolidation, and special finding/pleural lesion, as well as for perfusion abnormalities. The sum of morphological findings results in the \ac{MRI} morphology score, perfusion abnormalities comprise the \ac{MRI} perfusion score, and the sum of both results in the \ac{MRI} global score, ranging from 0 to 72 (Figure 2.3).

\subsection{Chest X-ray} 
All chest x-rays were captured in either the posterior–anterior or anterior–posterior projection. Specifically, chest x-ray examinations before and under \ac{ETI} therapy were assessed employing the modified Chrispin-Norman score without lateral view by the same reader (MOW) (\cite{cn_1,cn_2}).

The modified Chrispin–Norman score grades \ac{CF} lung damage on a single frontal film. The image is split into four lung quadrants, and four radiographic features — bronchial line (tram-track) shadows, ring shadows, mottled (nodular) shadows, and large soft shadows signifying consolidation or atelectasis. Each feature is rated in each quadrant on a three-point scale (0 = absent, 1 = mild, 2 = marked), yielding up to 32 points.

\section{Image Preprocessing} 
\label{sec:methods:methods_four}

Image preprocessing primarily involves two steps: chest x-ray size normalization and \ac{ROI} selection. \ac{FFT} was employed for image resizing. \ac{ROI} selection consists of two sub-tasks—lung region cropping and distinct lung region identification. Both sub-tasks were performed using two trained nnU-Net models (\cite{33,51}) (Figure 2.4). 

\begin{figure}
    \centering
    \includegraphics[width=.8\linewidth]{figures/fluss_scoring.drawio.png}
    \caption[Chest x-ray preprocessing]{\textbf{Workflow for chest x-ray preprocessing - from ROI selection to lung halves segmentation.}}
    \label{fig:preprocessing}
\end{figure}

\subsection{Chest X-ray \ac{ROI} Selection}
\begin{figure}
    \centering
    \includegraphics[width=.8\linewidth]{figures/fluss_roiselection.drawio.png}
    \caption[\ac{ROI} selection for chest x-ray]{\textbf{Lung \ac{ROI} selection remove the non-patient area to zoom in the lung region.}}
    \label{fig:roi_select}
\end{figure}

A large part of the chest x-ray contained non-patient areas including the artificial marker, black box and computer window. Those irrelevant areas will influence the next step - the lung halves segmentation process. An automatically \ac{ROI} selection process has been designed with help of nn-Unet. The network is trained with 12 human selected \ac{ROI} bounding box, then the trained network was used to automated select the \ac{ROI} of the rest 600 chest x-rays (Figure 2.5).

\subsection{Lung Halves Segmentation in Chest X-ray}
\begin{figure}
    \centering
    \includegraphics[width=.8\linewidth]{figures/optimizing.drawio.png}
    \caption[Lung halves segmentation]{\textbf{Optimized lung halves segmentation fit better to the pediatric patient in our dataset.} Approach 1 represent to the optimization with histogram adjust technique while approach 2 the adjustment of training dataset's age bias by additional chest x-rays.}
    \label{fig:lung_segment}
\end{figure}

Initially, an nn-UNet was trained on the open-source Montgomery County chest x-ray set (138 frontal chest x-ray, 17 from patients < 18 years). Performance was evaluated on 24 of our own films — six drawn from each pediatric age band (0–1, 1–6, 6–12, and 12–19 years). Through qualitative and quantitative analysis, three pre-processing schemes (global histogram equalisation, contrast-limited adaptive histogram equalisation (CLAHE), and simple intensity remapping) has been evaluated to boost segmentation quality. Next, a radiologist (LW) manually delineated 30 paediatric chest x-rays from our archive. Combining these with the Montgomery images a new nn-UNet was retrained (Fig. 2.6).

\section{Hybrid AI Scoring Model} 
\label{sec:methods:methods_six}
\begin{figure}
    \centering
    \includegraphics[width=1\linewidth]{figures/experimentaldesign.png}
    \caption[Hybrid AI scoring model]{\textbf{The design of the hybrid \ac{AI} scoring model to predict the visual \ac{MRI} scoring system by combining chest x-ray and \ac{PFT}.}}
    \label{fig:enter-label}
\end{figure}

This study was designed in two parts. First, a ResNet-50 was employed to predict each item of the visual \ac{MRI} score from a single frontal chest x-ray. Second, to provide an independent backup to the chest x-ray–based predictions, \ac{PFT} parameters were transformed into a 13-level quantitative scale and classified them with a centroid-based algorithm.

\subsection{Deep Learning Chest X-ray-predicted \ac{MRI} Score System}
Since the \ac{MRI} scoring system as reference is at a lobar level, it is possible to treat the score items in lung lobes independently. But due to the limited lobar information on chest x-rays, paired chest x-ray lung halves and corresponding visual \ac{MRI} scores can be generated, and have been used to predict the target scoring system.

\subsubsection{ResNet-50 Training}
A modified ResNet-50 architecture (\cite{34}) was trained on a single NVIDIA RTX 3060 GPU to predict visual \ac{MRI} scores from corresponding frontal chest x-rays. The network was fitted on the training set (chest x-ray images paired with \ac{MRI} labels), tuned on a validation set to select the best checkpoint, and finally evaluated on an independent test set. Output of the model are the lobar level \ac{MRI} scores for bronchiectasis/wall thickening, sacculation/abscess, mucus plugging, consolidation, special findings, and perfusion in one-hot format. Further details with code are provided in the supplement. 

\paragraph{Modified architecture}
The model starts with an $11{\times}11$ convolution followed by $3{\times}3$ max-pooling, then passes through the four standard ResNet stages of residual blocks, and ends with global average pooling and a fully connected layer for regression. Further details with code are provided in the supplement. 

\paragraph{Input and output}
Because the left and right lungs were analyses separately, each input is a $512{\times}512$-pixel chest x-ray with only one lung half. Instance-based min-max interval adjustment and z-score normalization have been performed during training. The network outputs a $3\times6$ matrix: three lobes (upper, middle/lingula, lower) by six pathological features, yielding lobar-level cystic-fibrosis scores for each lung half. Further details with code are provided in the supplement. 

\paragraph{Loss function}

To capture both point‐wise accuracy and agreement with the summed visual \ac{MRI} scores, a \emph{combined loss} was minimized:

\begin{equation}
\boxed{%
L_{C}= \alpha\,L_{E} + \beta\,L_{P} + \gamma\,L_{\rho}}, 
\qquad
\alpha,\beta,\gamma>0,\;
\alpha+\beta+\gamma = 1 .
\end{equation}

\noindent
Here

\begin{itemize}
  \item $L_{E}$ is the \textbf{cross‐entropy loss}  
        \begin{equation}
        L_{E}= -\frac{1}{N}\sum_{i=1}^{N}\Bigl[
        y_{i}\ln x_{i} + (1-y_{i})\ln(1-x_{i})
        \Bigr],
        \end{equation}

  \item $L_{P}$ is the \textbf{Pearson‐correlation loss}  
        \begin{equation}
        L_{P}= 1-\operatorname{corr}_{\mathrm P}(x,y)
             = 1-\frac{\displaystyle\sum_{i=1}^{N}(x_i-\bar x)(y_i-\bar y)}
                       {\sqrt{\displaystyle\sum_{i=1}^{N}(x_i-\bar x)^{2}}
                        \sqrt{\displaystyle\sum_{i=1}^{N}(y_i-\bar y)^{2}}},
        \end{equation}

  \item $L_{\rho}$ is the \textbf{Spearman‐correlation loss}.  
        Let $r_i=\operatorname{rank}(x_i)$ and $s_i=\operatorname{rank}(y_i)$, then  
        \begin{equation}
        L_{\rho}= 1-\operatorname{corr}_{\mathrm S}(x,y)
                = 1-\frac{\displaystyle\sum_{i=1}^{N}(r_i-\bar r)(s_i-\bar s)}
                          {\sqrt{\displaystyle\sum_{i=1}^{N}(r_i-\bar r)^{2}}
                           \sqrt{\displaystyle\sum_{i=1}^{N}(s_i-\bar s)^{2}}}.
        \end{equation}
\end{itemize}

\noindent
The weights $\alpha$, $\beta$, and $\gamma$ were tuned on the validation set; the final model used $\alpha = 0.5$, $\beta = 0.3$, and $\gamma = 0.2$, placing greatest emphasis on classification error while still enforcing both linear and ordinal agreement with the \ac{MRI} reference scores. Further details with code are provided in the supplement. 

\paragraph{Optimizer and Schedule}
The AdamW optimizer is a variant of the widely used Adam optimizer. It is designed to improve the training of deep neural networks by incorporating decoupled weight decay regularization. Unlike traditional Adam, AdamW separates weight decay from the gradient-based update step, leading to better generalization and training stability (\cite{adam}).

Effectively combined with AdamW optimizer, a step decay schedule progressively decreases the learning rate at predefined intervals during training. By reducing the learning rate at strategic epochs, the model can converge more precisely toward optimal minima, enhancing final model accuracy and robustness. The best initial learning rate for our model was at 0.0003. Further details with code are provided in the supplement. 

\subsubsection{Visualization of Activation Maps from ResNet-50}
To visualize the attention area of deep learning model, \ac{Grad-CAM} was employed \cite{52}. First, the pretrained ResNet-50 was run on all test-set chest x-ray. For every chest x-ray, the corresponding \ac{Grad-CAM} attention maps were computed from the fourth convolutional layer — the layer whose features feed the network’s lobar predictions. Finally, the attention maps were spatially aligned, normalized, and averaged across the entire test cohort, yielding a composite activation map highlighting the common regions the network focuses on when estimating lobar scores. Further details with code are provided in the supplement. 

\subsection{Centroid-based PFT-predicted \ac{MRI} Perfusion Score}
In this study, the centroid classifier was applied to cluster \ac{PFT} into 13 distinct groups. The centroid classifier method is a non-parametric approach classifying new observations based on their similarity to existing cases. Formally, for each new data point, the algorithm identifies its nearest centroid from the training set according to a chosen distance metric (Euclidean distance in this study). The new data point is then assigned to the cluster of nearest centroids. By leveraging centroid classifier, a data-driven clusters of lung function testing features was created, facilitating the differentiation of patients’ respiratory patterns into interpretable and clinically meaningful subgroups. However, clinical data can be more complex than a single metric: our dataset contained (1) records with both \ac{ppFEV1} and \ac{LCI}, (2) records with either only \ac{ppFEV1}, or (3) only \ac{LCI}. We therefore trained and tested separate centroid classifier models for each of these three scenarios, allowing users to choose the most appropriate model based on the available data. Further details with code are provided in the supplement. 

\subsection{Hybrid-predicted Score Combine Information from Chest X-ray and Pulmonary Function Tests}
Finally, both approaches were integrated into a hybrid deep learning model, where the \ac{MRI} score serves as both the baseline and the prediction target. The primary deep learning framework (ResNet-50) is supplemented by the centroid classifier, which is used to predict the \ac{MRI} perfusion score when at least one lung function metric (\ac{LCI} or \ac{ppFEV1}) is available. Consequently, the results presented include only cases where the \ac{MRI} score corresponds with both the chest x-ray and at least one \ac{PFT} metric.

\section{Statistical Analyses} 
\label{sec:methods:methods_eight}
3-class F1 score has been chosen to evaluate the quality of prediction at the lobar level, which can be interpreted as follows: $ 0.10 - 0.50 = \text{not good}, \quad 0.50 - 0.80 = \text{ok}, \quad 0.80 - 0.90 = \text{good}, \quad 0.90 - 1.00 = \text{very good}.$

To evaluate the agreement between the predicted score and \ac{MRI} scores at the lobar level, linear weighted Cohen’s Kappa has been employed and can be interpreted as follows: $\leq 0 = \text{no agreement}, \quad 0.01 - 0.20 = \text{none to slight}, \quad 0.21 - 0.40 = \text{fair}, \quad 0.41 - 0.60 = \text{moderate},0.61 - 0.80 = \text{substantial}, \quad 0.81 - 1.00 = \text{almost perfect agreement}.$ The loss function, combining cross-entropy and correlation loss, has been chosen to quantify the difference between predicted and target values.

The spearman rank correlation coefficient $\rho$ was calculated to measure agreement between predicted summary scores and \ac{MRI} summary scores, and interpreted as follows: $0.10 - 0.39 = \text{weak}, \quad 0.40 - 0.69 = \text{moderate}, \quad 0.70 - 0.89 = \text{strong}, \quad 0.90 - 1.00 = \text{very strong}$ (\cite{35}). Comparison between groups was performed by Mann-Whitney-U test and score changes in patients with \ac{CF} were assessed by analysis of variance (ANOVA) on ranks and Wilcoxon signed rank test. P < 0.05 was considered statistically significant. Measures to account for multiple comparison, e.g. Bonferroni-Holm method, was applied as appropriate.

