# Fine-tune
To fine tune we did the following

*Aug16_12-21-44_theBeans:* `fedsim-cli fed-tune --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:../../../data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:Real:0.04-0.15 --n-iters 10 --skopt-random-state 10 -r 4000`

# Final run
We got the best value with `lr=0.13`, therefore we relunch the experiment with this value for 10k rounds:

*Aug16_18-56-48_theBeans:* `fedsim-cli fed-learn --criterion CrossEntropyLoss log_freq:10 -n 1000 -d BasicDataManager num_partitions:1000 dataset:cifar100 rule:dir label_balance:0.3 root:../../../data/ save_dir:partitions -m cnn_cifar100 --batch-size 50 --test-batch-size 75 --global-score Accuracy log_freq:100 split:test --global-score CrossEntropyScore log_freq:100 split:test --epochs 5 --local-optimizer SGD weight_decay:0.001 lr:0.13 -r 10000`
