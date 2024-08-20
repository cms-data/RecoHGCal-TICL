# Data for the CMSSW [RecoHGCal/TICL](https://github.com/cms-sw/cmssw/tree/master/RecoHGCal/TICL) package

### Quicklinks

- TICL:
  - [Reconstruction example](http://hgcal.web.cern.ch/hgcal/Reconstruction/TICL/)
- TensorFlow:
  - [C++ docs](https://www.tensorflow.org/api_docs/cc)
  - [CMSSW interface](https://gitlab.cern.ch/mrieger/CMSSW-DNN)
  - [PhysicsTools/TensorFlow](https://github.com/cms-sw/cmssw/tree/master/PhysicsTools/TensorFlow)

### Models

- `tf_models/energy_id_v*.pb`: TensorFlow model for trackster energy regression and particle ID.
  - `v0`: Simple CNN-based approach. The neutral pion, neutral hadron, ambiguous and unknown probabilities are set to a constant value of 0. See the [talk at the Reco/AT meeting](https://indico.cern.ch/event/841640/contributions/3534140/attachments/1896780/3129591/2019-08-23_rieger_hgcal_ticl_eid.pdf) for more info. Input and output tensors:
    - `"input"`: Input tensor with dimension `batch x 50 (layers) x 10 (clusters) x 3 (features)`.
    - `"output/id_probabilities"`: Output tensor with dimension `batch x 8` representing particle ID "probabilities" (from a softmax output). The probabiltities refer to photon, electron, muon, neutral pion, charged hadron, neutral hadron, ambiguous and unknown cases (in that order).
    - `"output/regressed_energy"`: Output tensor with dimension `batch x 1` representing the regressed energy value for the trackster.
 - `superclustering/`: ONNX models (from PyTorch) for superclustering of electrons. 
   - `superclustering/supercls_v2p1.onnx`: DNN, inputs features computed from pairs of tracksters (uses inputs defined in `SuperclusteringDNNInputV2` in `RecoHGCal/TICL/interface/SuperclusteringDNNInputs.h`). Input format : `batch x 17 (features)`. Outputs score (dimension `batch`) giving "probability" that the sub-leading trackster is a bremmstrahlung photon of the leading trackster. Optimal working point : 0.3.
   - `superclustering/regression_v1.onnx`: DNN for supercluster energy regression. Input format : `batch x 8 (features)`. Output : `batch x 1` (supercluster regressed energy). Used in `RecoHGCal/TICL/plugins/EGammaSuperclusterProducer.cc`.
