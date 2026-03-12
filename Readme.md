# REANA example - FCC-hh Full Simulation Tracker Performance study

## About

This example reproduces one curve of figure 7.12 from the Conceptual Design
Report (CDR) of the hadronic Future Circular Collider (FCC-hh)
<http://cds.cern.ch/record/2651300> .

<img src="docs/Trk_dpTOverpTFinalCorrWithRiemanFit.png" alt="CDR figure 7.12"
width="60%">

Fig. 7.12 compares the momentum resolution of the proposed track detector model
for two magnet options. The resolution is calculated using different
approximations. Here we reproduce the full simulation result that simulates
single muons in Geant4 and fits them using the Riemann fit, all using the FCC
software framework FCCSW.

The first step is the full detector simulation of single muons. This should take
around 30 mins on a machine with at least 4GB of RAM. The output root file
contains information on the original particles and the corresponding tracker
hits in the FCC event data model. We can inspect the number of hits per track
with the script `'numHitsPerTrack.C("muons_for_seeding_discrete_pt.root")'`, for
example.

 <img src="docs/numHitsPerTrack.png" alt="numHitsPerTrack" width="60%">

The final step is fitting the tracker hits with a Riemann fit, implemented in
the `tricktrack` seeding library, which is again integrated in the FCC
Framework. This yields the reconstructed tracks, which can be plotted with the
python script `plot_single_particle_resolutions.py`:

<img
src="docs/SinglePartRes_single_particle_resolutions_nevents05000_pt_res_div_binned.png"
alt="finalResolutions" width="60%">

The pink curve corresponds to the one in figure 7.12. While the Riemann Fit
cannot correctly estimate the resolutions for low momentum tracks, whose errors
are dominated by scattering, the agreement of the high momentum tracks with the
analytical methods is good.

## How to run the example on REANA cloud

[REANA](http://www.reana.io/) is a reproducible and reusable research data
analysis platform that permits to run structured computational workflows on
remote compute clouds.

1. First, set up `reana-client` that will be used to submit the analysis to
   REANA cloud:

   ```console
   $ # on lxplus.cern.ch, you can use a provided environment:
   $ source ~simko/public/reana/bin/activate

   $ # on your laptop, you can create one as follows:
   $ virtualenv ~/.virtualenvs/reana
   $ source ~/.virtualenvs/reana/bin/activate
   $ pip install reana-client  # FIXME: needs a new release on PyPI
   ```

2. Second, configure the client to connect the wanted REANA instance where the
   analysis will run, using the access token that was provided to you by the
   REANA instance administrators:

   ```console
   $ export REANA_SERVER_URL=http://reana.cern.ch/
   $ export REANA_ACCESS_TOKEN=XXXXXXX
   ```

3. Third, you can now use `reana-client` to create new workflow, upload input
   code, start the execution, check its status, logs, produced output files and
   download the plots:

   ```console
   $ reana-client create -n fcchh-fullsim
   $ export REANA_WORKON=fcchh-fullsim
   $ reana-client upload
   $ reana-client start
   $ reana-client status
   $ reana-client logs
   $ reana-client list
   $ reana-client download numHitsPerTrack.png
   ```

For more information on how to interact with the REANA platform, please see
[reana-client documentation](https://reana-client.readthedocs.io/).
