# FB-ISC


## exp
- v1: simsiam
- v2: simsiam (aug片方だけ)
- v3: arcface
- v4: だんだんsample sizeを増やしていく
- v5: triplet loss
- v6: with miner
- v7: contrastive loss
- v8: Augly
- v9: Augly fixed
- v10: XBM
- v11: Bipartite contrastive loss
- v12: XBM only enqueue noaug
- v13: Bipartite XBM
- v14: XBM + NTXent
- v15: XBM + SignalToNoiseRatioContrastiveLoss
- v16: v10 + w/o bn
- v17: v10 + deeper head
- v18: v10 + different backbone
- v19: v18 + amp
- v20: v19, no sync bn
- v21: v20 + BN shuffle


python v8.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 1 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 32 --ncrops 4 \
  --gem-p 3.0 --gem-eval-p 4.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 320 --sample-size 100000 \
  ../train_subset
CUDA_VISIBLE_DEVICES=2 python v8.py \
  -a dm_nfnet_f0 \
  --batch-size 128 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 4.0 \
  --weight ./v8/train/checkpoint_0000.pth.tar \
  --input-size 320 \
  --eval-subset \
  /mnt/sdb/yokoo/fbisc/input/
python v8.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 10 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 320 --sample-size 1000000 \
  ../input/training_images
python v8.py \
  -a dm_nfnet_f0 \
  --batch-size 256 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v8/train/checkpoint_0004.pth.tar \
  --input-size 320 \
  ../input/
python v10.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 1 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../train_subset
CUDA_VISIBLE_DEVICES=2 python v10.py \
  -a dm_nfnet_f0 \
  --batch-size 128 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v10/train/checkpoint_0000.pth.tar \
  --input-size 256 \
  --eval-subset \
  ../input/
python v18.py \
  -a tf_efficientnetv2_s_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 1 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../train_subset
CUDA_VISIBLE_DEVICES=2 python v18.py \
  -a tf_efficientnetv2_s_in21k \
  --batch-size 128 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v18/train/checkpoint_0000.pth.tar \
  --input-size 256 \
  --eval-subset \
  ../input/
python v19.py \
  -a tf_efficientnetv2_s_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 5 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 128 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 10000 \
  ../input/training_images/
python v19.py \
  -a tf_efficientnetv2_s_in21k \
  --batch-size 512 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v19/train/checkpoint_0004.pth.tar \
  --input-size 256 \
  --eval-subset \
  ../input/
python v19.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 5 \
  --lr 0.02 --wd 1e-5 \
  --batch-size 128 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 10000 \
  ../input/training_images/
python v19.py \
  -a dm_nfnet_f0 \
  --batch-size 512 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v19/train/checkpoint_0004.pth.tar \
  --input-size 256 \
  --eval-subset \
  ../input/

python v20.py \
  -a tf_efficientnetv2_m_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 1 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../input/training_images/
python v20.py \
  -a tf_efficientnetv2_m_in21k \
  --batch-size 1024 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 6.0 \
  --weight ./v20/train/checkpoint_0000.pth.tar \
  --input-size 256 \
  --eval-subset \
  ../input/
python v19.py \
  -a tf_efficientnetv2_s_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 10 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 384 --sample-size 1000000 --memory-size 4096 \
  ../input/training_images/
CUDA_VISIBLE_DEVICES=2 python v19.py \
  -a tf_efficientnetv2_s_in21k \
  --batch-size 256 \
  --mode extract \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --weight ./v19/train/checkpoint_0009.pth.tar \
  --input-size 384 \
  ../input/

python v21.py \
  -a tf_efficientnetv2_s_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 5 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 512 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 10000 \
  ../input/training_images/
for epoch in `seq 0 4`; do
  python v21.py -a tf_efficientnetv2_s_in21k --batch-size 512 --mode extract --gem-p 3.0 --gem-eval-p 5.0 --weight ./v21/train/checkpoint_000${epoch}.pth.tar --input-size 256 --eval-subset ../input/
done

for epoch in `seq 0 4`; do
  python v19.py -a dm_nfnet_f0 --batch-size 512 --mode extract --gem-p 3.0 --gem-eval-p 5.0 --weight ./v19/train/checkpoint_000${epoch}.pth.tar --input-size 256 --eval-subset ../input/
done

python ../scripts/eval_metrics.py v2/extract/fb-isc-submission.h5 ../input/public_ground_truth.csv

384x384:
{
  "average_precision": 0.15055434775303866,
  "recall_p90": 0.06952514526147065
}
v8, 320x320
{
  "average_precision": 0.2829796970097667,
  "recall_p90": 0.07313163694650371
}


{
  "average_precision": 0.4242279033736717,
  "recall_p90": 0.29252654778601483
}
python v8.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 1 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 4.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 320 --sample-size 100000 \
  ../train_subset

{
  "average_precision": 0.4604716294783043,
  "recall_p90": 0.33099579242636745
}
python v10.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 1 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 320 --sample-size 100000 --memory-size 4096 \
  ../train_subset

{
  "average_precision": 0.45031799005000156,
  "recall_p90": 0.2785013023442196
}
python v10.py \
  -a dm_nfnet_f0 \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 \
  --epochs 1 \
  --lr 0.01 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../train_subset

{
  "average_precision": 0.49208326754843634,
  "recall_p90": 0.3698657583650571
}
python v18.py \
  -a tf_efficientnetv2_s_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 1 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../train_subset

{
  "average_precision": 0.4996100416157455,
  "recall_p90": 0.3742736926467642
}
python v19.py \
  -a tf_efficientnetv2_m_in21k \
  --dist-url 'tcp://localhost:10001' --multiprocessing-distributed --world-size 1 --rank 0 --seed 7 \
  --epochs 1 \
  --lr 0.1 --wd 1e-5 \
  --batch-size 64 --ncrops 2 \
  --gem-p 3.0 --gem-eval-p 5.0 \
  --pos-margin 0.0 --neg-margin 1.1 \
  --input-size 256 --sample-size 100000 --memory-size 4096 \
  ../input/training_images/
