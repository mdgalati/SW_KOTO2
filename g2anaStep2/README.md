Executable codes to reconstruct $\pi^0$ from the output file of myfsimIBBHPV

- Reconstruct $\pi^0$ and get $z_{vtx}$
- Add kinematic variables for the analysis

Location of the library: `/sw/koto2/e14lib/201605v6.5/e14/sources/E14RecLibrary`

Source file of g2_ana class: `/group/had/koto2/fsim/g2anaStep2/src` 

Header file of g2_ana class: `/group/had/koto2/fsim/g2anaStep2/g2anaStep2`

```bash
/group/had/koto2/fsim/g2anaStep2/bin/g2anaStep2 KLpi0ee_fsim.root KLpi0ee_g2ana.root 175
```

- **Contents of TTree in g2anaStep2**
    
    EventID: Event ID of g2anaStep2 (starts from 0)
    
    OrigEventID: EventID of myfsimBHCV (starts from 1)
    
    MCEventID : EventID of gsim4test
    
    Pi0Number : # of reconstructed π0
    
    Pi0Mome : Reconstructed π0 momentum
    
    Pi0Pos : Reconstructed π0 Position
    
    nHitCluster: # of clusters in the calorimeter
    
    ClusterXXX : the same as those in myfsimBHCV
    
    nHitVeto: # of clusters in the calorimeter
    
    VetoXXX: the same as those in myfsimBHCV
    
    EventWeight: the same as that in myfsimBHCV
    
    CutCondition: flag for kinematical cuts
    
    0 bit: Gamma energy Cut
    
    1 bit: Fiducial cut
    
    2 bit: Vertex Cut
    
    3 bit: Pt Cut
    
    4 bit: Projection Angle Cut
    
    5 bit: Gamma distance cut 
    
    6 bit: Total Energy Cut
    
    7 bit: Energy Theta Cut
    
    8 bit: Energy ratio cut
    
    RecEvenOdd: ( Fusion clusters exist : -1, ParentID of two gammas is the same : 1, other 0)
    
    RecVertZ : Z position of the reconstructed π0 vertex
    
    RecPi0Pt : Reconstructed π0 Pt
    
    RecVertZsig2: χ2 value for the π0 vertex reconstruction
    
    RecPi0Mass : Reconstructed π0 mass
    
    RecPi0E : Reconstructed π0 energy
    
    RecPi0Px,Py,Pz: Px, Py and Pz of the reconstructed π0 momentum
    
    RecPi0Vx,Vy,Vz: Vx, Vy and Vz of the reconstructed π0 vertex → Vx and Vy always 0?
    
    gE : photon energy in the calorimeter (same as values of ClsuterHitTotE)
    
    gR : Distance between the beam axis and the gamma hit position
    
    gD : Max( |ClusterHitPos[][0]|, |ClusterHitPos[][1])
    
    gTheta: Theta angle of the incident gamma
    
    ProjAng: Projection angle of the two gammas on the calorimeter
    
    Dist : Distance between two gammas on the calorimeter
    
    Etot: total energy of the two gammas on the calorimeter
    
    Eratio: Energy ratio of two gamma Min(gE[0],gE[1])/Max(gE[0],gE[1])
    
    CBAREt: Not used (The value is always 0)
    
    TrueMom: Not used (The value is always 0)
    
    thetaPi0: Reconstructed theta angle of the incident gammas under the π0 mass assumption
    
    phiPi0 : Reconstructed phi angle of the incident gammas under the π0 mass assumption  
    
    thetaKL: Reconstructed theta angle of the incident gammas under the KL mass assumption 
    
    phiKL : Reconstructed phi angle of the incident gammas under the KL mass assumption
    
    thetaCM: Reconstructed theta angle of the incident gammas in the center of mass system under the KL mass assumption
    
    thetaPhi: Reconstructed phi angle of the incident gammas in the center of mass system under the KL mass assumption
