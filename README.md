This repository contains the result of tuning centralized Federated Learning algorithm using [FedSim](fedsim.varnio.com).

## Tensorboard in Colab
To see the results in colab
* go to https://colab.research.google.com/
* select GitHub tab (close if any any pop-up window)
* enter the repo's url: https://github.com/varnio/tune-centralized-fl.git
* under repository filed, select `varnio/tune-centralized-fl`
* select `colab_tensorboard.ipynb`

At this point the visualization notebook should show up. Run the initialization part to clone the run files and then run the rest to see visualization for each experiment.


## Tune on Colab
* go to https://colab.research.google.com/
* make a new notebook
* enter the following lines:

```bash
! git clone https://github.com/varnio/tune-centralized-fl.git
! pip install fedsim
```
now run your experiments!