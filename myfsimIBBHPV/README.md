```bash
myfsimIBBHPV <in.root> <out.root> <vth> <vthBHPV> [CSI] [NCC] [CSIVeto]
```

where:

- `vth`: Threshold for Barrel (MeV)
- `vthBHPV`: Threshold for BHPV (PE)

```bash
/group/had/koto2/fsim/myfsimIBBHPV/bin/myfsimIBBHPV KLpi0ee.root KLpi0ee_fsim.root 1 6
```

- **`/group/had/koto2/fsim/myfsimIBBHPV/myfsimIBBHPV.cc`**
    
    Headers: `/sw/koto2/e14lib/201605v6.5/e14/sources/sim/fsim/E14Fsim/E14Fsim/`
    
    Source files: `/sw/koto2/e14lib/201605v6.5/e14/sources/sim/fsim/E14Fsim/src/`
    
    - **Barrel threshold**
        
        Based on `vth`: `mb->setupThreshold(vth);`
        
        Where: `E14FsimZSBInefficiency* mb = new E14FsimZSBInefficiency();` → this loads a model (empirical curve) that decides whether small-energy hits in CsI are lost.
        
    - **BHPV threshold**
        
        `E14FsimFunction::getFunction()->setBHPVConfiguration(2502,vthBHPV);` → should not change anything
        
        2502 means “25 layers, optical”. see: `/sw/koto2/e14lib/201605v6.5/e14/sources/sim/gsim4/GsimE14Detector/src/GsimE14BHPV.cc:1996`
        
        Ch aerogel + optical box
        
- **Inefficiencies functions**
    - The **photon** inefficiency function for CsI crystals (`csiineff` in [E14FsimFunction.cc](http://e14fsimfunction.cc/)) was obtained from data
        
        $$
        \text{csiineff}(x) =
        \begin{cases}
        p_0 \cdot 2^{p_1} & \text{if } x > 2,\\[2mm]
        \min\big(1,\, p_0 \cdot x^{p_1}\big) & \text{if } x \le 2,
        \end{cases}\\
        
        \text{where } p_0 = 3.5409 \times 10^{-7}, \; p_1 = -2.27731, \; x \text{ is energy in GeV.}
        $$
        
        ![Screenshot 2025-11-24 at 16.28.13.png](attachment:ef92ad90-8059-4754-8662-c87f60f323d1:Screenshot_2025-11-24_at_16.28.13.png)
        
    - The **charged pion** inefficiency function for charged veto counters (`newCVineffPi` in [E14FsimFunction.cc](http://e14fsimfunction.cc/)) was obtained from MC simulation, but the additional scale factor was applied to adjust the results of experimental data.
        
        $$
        \text{iPID} = \begin{cases}0 & \text{if PID } = -211 (\pi^-),\\1 & \text{otherwise},\end{cases}
        \\[6pt]
        (p_0, p_1, p_2, p_3, p_4, p_5) = m_{\mathrm{CVineffPar}}[\text{iPID}].
        \\[12pt]
        \text{Exponential term: } \quad\mathrm{ineff}_{\exp}(P) = p_0 + p_1 \, e^{\,p_2 P}
        \\[10pt]
        \text{Gaussian term: } \quad A = P - p_4, \qquad\mathrm{ineff}_{\mathrm{gaus}}(P) = p_3 \, e^{-{A^2}/{2 p_5^2}}
        \\[12pt]
        \text{Total inefficiency: } \quad\mathrm{ineff}(P) =\min\!\left( 1,\;\mathrm{ineff}_{\exp}(P) + \mathrm{ineff}_{\mathrm{gaus}}(P)\right)
        $$
        
        ![Screenshot 2025-11-25 at 16.25.44.png](attachment:6e498e14-c184-4270-8e6d-ef1f8fc0a278:Screenshot_2025-11-25_at_16.25.44.png)
        
        there’s a resonance around 300 MeV → to be checked
        
        pi- is more easily captured at low energies
        
        iPID to be checked, it should be just for pions. check if there are other inefficiencies function for leptons
        
    - The other inefficiency functions were obtained based on the MC studies.
- **Smearing functions**
    
    **Energy**:   `csiEnergyRes` in [E14FsimFunction.cc](http://e14fsimfunction.cc/)  (1%/E + 2%/sqrt(E) unit of E is GeV)
    
    **Position**:  `csiPosRes`  in  [E14FsimFunction.cc](http://e14fsimfunction.cc/)  (5mm/sqrt(E) unit of E is GeV)
    
    Those functions are used in [E14FsimCluster.cc](http://e14fsimcluster.cc/) to smear the energy and position of the photons hitting the calorimeter.
    

---

Executable codes to analyse fast simulation data obtained from gsim4test.

- Remove events if # of photons hitting the calorimeter is less than 2.
- Count all possibilities to detect 2 clusters in the calorimeter for the photons hitting the calorimeter (detection  probability, mis-detection probability, probability merged with a photon nearby hit) by adding event weight.
- Get the positions and energies for gammas that are selected as detected clusters in the calorimeter.
- Evaluate the mis-detection probability for particles hitting veto detectors by using inefficiency functions
- **Contents of TTree in myfsimBHCV**
    - KDecayMome : Momentum of KL (MC true information)
    - KDecayPos : Decay position of KL (MC true information)
    - KDecayTime : Time when KL decays
    - Pi0Number : # of π0 generated from KL
    - Pi0Pos : Generation positions of π0 (MC true information)
    - Pi0Mome : Momentum of π0 (MC true information)
    - DecayDaughterNumber : # of daughter particles from KLs
    - DecayDaughterPID : PDG particle ID for daughter particles (MC true information)
    - DecayDaugherMass : Mass of daughter particles (MC true information)
    - DecayDaugherEkin : Kinetic energy of daughter particles (MC true information)
    - DecayDaugherP : Momentum of daughter particles (MC true information)
    - eventID : EntryID of TTree
    - gsimEventNumber: EventID of gsim4test
    - mcEventID: EventID of gsim4test
    - nHitCluster: # of photons hitting gammas (always 2)
    - ClusterHitPos : gamma’s position on the calorimeter
    - ClusterHitTotE : gamma’s energy on the calorimeter
    - ClusterHitAng : always 0
    - ClusterHitTime: gamma’s hit timing at the calorimeter
    - ClusterHitThisID: TrackID of gammas contained in the cluster
        
        If two gammas are contained in the cluster, -2000- 100* (TrackID of Second Gamma) - (TrackID of First Gamma)
        
        If more than 2 gammas are contained in the cluster, -3000- 100* (TrackID of Second Gamma) - (TrackID of First Gamma)
        
    - ClusterHitParentID: Track ID of $\pi^0$ that decayed to gammas contained in the cluster. (The same convention as ClusterHitThisID is used)
    - ClusterHitTrueP : True momentrum of the cluster
    - ClusterHitSigmaE : Values used to smear the cluster energy (MeV)
    - ClusterEffi : CalHitClusterWeight x CalIneffiGammaWeight
    - CalHitClusterWeight : Detection efficiency of two photons
    - CalIneffiGammaWeight: Detection inefficiency if more than two cluster hit the calorimeter
    - ClusterFusionProb : probability to make a fusion cluster by two  gamma nearby hits
    - ClusterWeight : ClusterEffi x ClusterFusionProb
    - nHitVeto: # of particles hitting veto detectors
    - VetoPID: PDG particle ID of the particle hitting veto detectors
    - VetoDetID: Veto detector ID
        
        NCC:2, CC04:4, CBAR:7, FBAR:8, CSI:9, BHPV: 10
        
        BHPV plays as BHCV if incident particles are charged particles
        
        CC04 plays as DCV if incident particles are charged particles
        
    - VetoPos : Hit position (Surface of veto detector, MC true information)
    - VetoMome: Momentum of the particles
    - VetoTime: Veto Timing (MC true information)
    - VetoIneffi: detection Inefficiency for the particle
    - DetVetoWeight: Inefficiency of each detector
    - VetoWeight: total detection inefficiency for the event
    - EventWeight: Weight of the event ( ClusterWeight x VetoWeight)
