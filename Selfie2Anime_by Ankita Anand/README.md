

## Installation


```bash
cd AttentionGAN/
```

This code requires PyTorch 0.4.1+ and python 3.6.9+. Please install dependencies by
```bash
pip install -r requirements.txt (for pip users)
```
or 

```bash
./scripts/conda_deps.sh (for Conda users)
```



## Dataset Preparation
Download the datasets using the following script.
```
sh ./datasets/download_cyclegan_dataset.sh selfie2anime
```
The selfie2anime dataset can be download [here](https://drive.google.com/file/d/1xOWj1UVgp6NKMT3HbPhBbtq2A4EDkghF/view).

## AttentionGAN Training/Testing
- Download the dataset using the previous script.

- Train the model:
```
sh ./scripts/train_attentiongan.sh
```
- To see more intermediate results, check out `./checkpoints/horse2zebra_attentiongan/web/index.html`.
- How to continue train? Append `--continue_train --epoch_count xxx` on the command line.
- Test the model:
```
sh ./scripts/test_attentiongan.sh
```


## Generating Images Using Pretrained Model
- You need download a pretrained model (e.g., Selfie2Anime) with the following script:
```
sh ./scripts/download_attentiongan_model.sh horse2zebra
```
- The pretrained model is saved at `./checkpoints/selfie2anime_pretrained/latest_net_G.pth`. 
- Then generate the result using
```
python test.py --dataroot ./datasets/horse2zebra --name horse2zebra_pretrained --model attention_gan --dataset_mode unaligned --norm instance --phase test --no_dropout --load_size 256 --crop_size 256 --batch_size 1 --gpu_ids 0 --num_test 5000 --epoch latest --saveDisk
```
The results will be saved at `./results/`. Use `--results_dir {directory_path_to_save_result}` to specify the results directory. Note that if you want to save the intermediate results and have enough disk space, remove `--saveDisk` on the command line.

- For your own experiments, you might want to specify --netG, --norm, --no_dropout to match the generator architecture of the trained model.

### Image Translation with Geometric Changes Between Source and Target Domains
For instance, if you want to run experiments of Selfie to Anime Translation. Usage: replace `attention_gan_model.py` and `networks` with the ones in the `AttentionGAN-geo` folder.

### Test the Pretrained Model 
Download data and pretrained model according above instructions.

`python test.py --dataroot ./datasets/selfie2anime/ --name selfie2anime_pretrained --model attention_gan --dataset_mode unaligned --norm instance --phase test --no_dropout --load_size 256 --crop_size 256 --batch_size 1 --gpu_ids 0 --num_test 5000 --epoch latest`

### Train a New Model
`python train.py --dataroot ./datasets/selfie2anime/ --name selfie2anime_attentiongan --model attention_gan --dataset_mode unaligned --pool_size 50 --no_dropout --norm instance --lambda_A 10 --lambda_B 10 --lambda_identity 0.5 --load_size 286 --crop_size 256 --batch_size 4 --niter 100 --niter_decay 100 --gpu_ids 0 --display_id 0 --display_freq 100 --print_freq 100`

### Test the Trained Model
`python test.py --dataroot ./datasets/selfie2anime/ --name selfie2anime_attentiongan --model attention_gan --dataset_mode unaligned --norm instance --phase test --no_dropout --load_size 256 --crop_size 256 --batch_size 1 --gpu_ids 0 --num_test 5000 --epoch latest`


