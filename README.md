
These scripts can be used to extract metrics from log and publish to Heatpipe(Kafka) or store as CSV

There are 2 workflows available as of writing this:
 1. **metrics** - to publish plan deviation metrics to topic **hp-sim_metrics-plan**
 2. **auto** - to publish auto/manual intervention flag for a log to **topic hp-sim_metrics-auto**

# Sample run
 1. **To publish plan deviation metrics to Heatpipe:** generate_sim_metrics.py --kafka   --log loglocator:674b3e26-0d03-d7dc-f804-42a6b41ba52d/navigator_rerecord/output_pf_amendment_1198938434 --minimum 1198938414 --maximum 1198938415 --workflow auto  --tag test --av_version 345_9_CI

 2. **To store auto metrics as CSV:** 
   * export file_path=/mnt/logs/YELLOW/av/2018.01/2018.01.03/KRYPTON0038/22.28.21_PHX_Canonical_TempeC_Auto/navigator_rerecord/output_shiv_amendment5_1199056212/output_shiv_amendment5_1199056212Index00000.hlog
   * generate_sim_metrics.py --csv --log $file_path -o /tmp/metrics.csv --minimum 1199056211 --maximum 1199056212 --workflow metrics --test


Available topics for Simulation metrics engineering:
1. hp-sim_metrics-plan
2. hp-sim_metrics-auto


# How to create new metric workflow
Refer [here](https://docs.google.com/document/d/1quaYgUo6XT-KWeRMV96tuGcq3cBdTLN4rpbLcVja7HI) for more details

1. Create uBlame team if not done already. Team name for Simulation metrics engineering is **atg-sim-metrics**
2. Go to uBlame and add a service alias. Service alias for Simulation metrics engineering is **SIM_METRICS**
3. Create a new topic. Refer [here](https://watchtower.uberinternal.com/watchtower/#topic=hp-sim_metrics-plan) for an existing topic
4. Add schema for the newly created topic

## Code changes to add new metric
1. Add new workflow name [here](https://code.int.uberatc.com/diffusion/AV/browse/master/source/mp_log_validation/python/analytics/workflow_names.py)
    . If you are modifying schema of existing topic, update this file with correct version.
2. Add new class for your metrics similar to this [file](https://code.int.uberatc.com/diffusion/AV/browse/master/source/mp_log_validation/python/analytics/log_auto.py)
3. Add extraction logic for your new metric similar to to this [file](https://code.int.uberatc.com/diffusion/AV/browse/master/source/mp_log_validation/python/analytics/workflows/auto.py)
