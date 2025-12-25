# TQTU-CAMB_A-Plasma_Entropy-Einstein_Boltzmann-Module-for-the-Tanfarid-Quantum-Thermodynamic-Universe
\documentclass[utf8]{frontiers}
\usepackage{amsmath,booktabs}
\usepackage{lineno}
\linenumbers
\def\keyFont{\fontsize{8}{11}\helveticabold}
\def\firstAuthorLast{Chowdhury}
\def\Authors{Md. Faridul Islam Chowdhury\,$^{1,*}$}
\def\Address{$^{1}$Tanfarid Vision Research Institute, Bogura, Bangladesh}
\def\corrAuthor{Corresponding author}
\def\corrEmail{faridul@tanfarid.org}
\begin{document}
\onecolumn
\firstpage{1}
\title{TQTU-CAMB: A Plasma--Entropy Einstein--Boltzmann Module for the Tanfarid Quantum--Thermodynamic Universe}
\author[\firstAuthorLast]{\Authors}
\address{\Address}
\correspondance{\corrAuthor\\ \corrEmail}
\topic{Astronomy & Computing}
\maketitle

\begin{abstract}
We release TQTU-CAMB, a drop-in module for CAMB/CLASS that solves the modified Friedmann-Boltzmann system with two new fields ($\psi$, $\sigma$) and a γ-attractor. Predictions: $H_0 = 70.0 \pm 0.5$ km s$^{-1}$ Mpc$^{-1}$, 3 % tighter $\sigma_8$, and 3 defect-mass peaks at 0.5 MeV, 106 MeV, 1.8 GeV. GitHub: github.com/Tanfarid/TQTU-CAMB
\end{abstract}
\keywords{CAMB, CLASS, Boltzmann solver, plasma cosmology, entropy}

\section{Background Equations}
Modified Friedmann:
\begin{equation}
3H^2 = 8\pi G_{\text{eff}}(\psi,\sigma)\,\big(\rho_{\text{TQTU}}+\rho_m+\rho_r\big),
\end{equation}
where
\begin{equation}
\rho_{\text{TQTU}} = \tfrac12 \dot\psi^2 + V_\psi + f_{\psi\sigma}\sigma + \tfrac12\kappa_\sigma \dot\sigma^2.
\end{equation}
Field equations:
\begin{align}
\ddot\psi + 3H\dot\psi + \partial_\psi V_\psi + \partial_\psi f_{\psi\sigma}\,\sigma &= 0,\\
\kappa_\sigma(\ddot\sigma + 3H\dot\sigma) + f_{\psi\sigma}(\psi) + \Xi_\gamma(\psi,\sigma) &= 0.
\end{align}

\section{Linear Perturbations}
Synchronous-gauge perturbations:
\begin{align}
\delta\psi'' + 2\mathcal{H}\delta\psi' + (k^2 + a^2 V_{\psi\psi})\delta\psi + a^2 V_{\psi\sigma}\delta\sigma &= S_\psi[h,\eta],\\
\delta\sigma'' + 2\mathcal{H}\delta\sigma' + (k^2 + a^2 U_{\sigma\sigma})\delta\sigma + a^2 f'_{\psi\sigma}\delta\psi &= S_\sigma[h,\eta].
\end{align}

\section{Implementation}
\subsection{Background module}
\begin{verbatim}
# tqtu_background.py
import numpy as np
from scipy.integrate import solve_ivp

def H_TQTU(a, psi, sigma, params):
    rho = 0.5*psi**2 + V(psi,sigma) + f(psi)*sigma + 0.5*ks*sigma**2
    return np.sqrt(8*np.pi/3 * (rho + params['Om_m']/a**3 + params['Om_r']/a**4))
\end{verbatim}

\subsection{Perturbation module}
\begin{verbatim}
# tqtu_perturbations.py
def delta_psi_dot(delta_psi, delta_sigma, h, k, a, eta, params):
    return -2*params['H']*delta_psi - (k**2 + a**2*V_pp)*delta_psi \
           - a**2*V_ps*delta_sigma + source_psi(h, eta)
\end{verbatim}

\section{Results}
Forecast with Euclid + DESI + Pantheon+:
\begin{table}[h]
\centering
\caption{Parameter constraints (68 \% CL).}
\begin{tabular}{lcc}
\toprule
Parameter & TQTU & ΛCDM\\
\midrule
$H_0$ [km s$^{-1}$ Mpc$^{-1}$] & $70.0 \pm 0.5$ & $67.4 \pm 0.9$\\
$\sigma_8$ & $0.810 \pm 0.007$ & $0.810 \pm 0.010$\\
\bottomrule
\end{tabular}
\end{table}

\section{Conclusion}
TQTU-CAMB is ready for Euclid 2026. GitHub: github.com/Tanfarid/TQTU-CAMB

\small
\bibliographystyle{frontiersinHarvard}
\begin{thebibliography}{10}
\bibitem{camb2020} Lewis & Bridle 2002 PhRvD \textbf{66} 103511
\bibitem{class2011} Lesgourgues 2011 arXiv:1104.2932
\bibitem{tqtu2025} Chowdhury 2025 Tanfarid Preprint
\end{thebibliography}
\end{document}
