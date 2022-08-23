## Fine-tune lr
To fine tune we did the following

*Aug16_12-21-44_theBeans:* `fedsim-cli fed-tune --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:../../../data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:Real:0.04-0.15 --n-iters 10 --skopt-random-state 10 -r 4000`

## Tuned lr
We got the best value with `lr=0.13`, therefore we relunch the experiment with this value for 10k rounds:

*Aug16_18-56-48_theBeans:* `fedsim-cli fed-learn --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:../../../data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:0.13 -r 10000`


## Fine-tune lr & gamma together

By default we apply a StepLR scheduling (with gamma=0.999) to decay the round to round learning rate. In this experiment we jointly tune gamma with the local leanring rate.

`fedsim-cli fed-tune --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:Real:0.04-0.15 --n-iters 20 --skopt-random-state 20 -r 2000 --r2r-local-lr-scheduler StepLR step_size:1 gamma:Real:0.98-0.9999`

## Tuned lr & gamma
It turns out that the best local leanring rate is 0.15 and the best gamma is 1.0 (no decay). In this experiment we run this config again.

`fedsim-cli fed-learn --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:0.15 -r 10000`

## Tuning for higher lr
Sicen 0.15 was the largest value of the range given to local learning rate for tuning, we might want to see how would it work for > 0.15. This experiments is done for this reason.

`fedsim-cli fed-tune --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:Real:0.15-0.25 --n-iters 20 --skopt-random-state 20 -r 2000 --r2r-local-lr-scheduler StepLR step_size:1 gamma:Real:0.98-0.9999`

The best values are again 0.15 and 1.0 for local learning rate and gamma, respectively.

## Final verdict

Comparing the first long run (local lr=0.13, gamma=0.999) and the second one (local lr=0.15, gamma=1.0) may indicate that learning slower would eventually work better. We shall repeat the long run experiments for more number of seeds to confirm.