## Evaluation Instruction for MiniGPT-v2

### Data preparation
Images download
Image source | Download path
--- | :---:
OKVQA| <a href="https://drive.google.com/drive/folders/1jxIgAhtaLu_YqnZEl8Ym11f7LhX3nptN?usp=sharing">annotations</a> &nbsp;&nbsp;  <a href="http://images.cocodataset.org/zips/train2017.zip"> images</a>
gqa | <a href="https://drive.google.com/drive/folders/1-dF-cgFwstutS4qq2D9CFQTDS0UTmIft?usp=drive_link">annotations</a> &nbsp;&nbsp;  <a href="https://downloads.cs.stanford.edu/nlp/data/gqa/images.zip">images</a> 
hateful meme |  <a href="https://github.com/faizanahemad/facebook-hateful-memes">images and annotations</a> 
iconqa |  <a href="https://iconqa.github.io/#download">images and annotation</a>
vizwiz |  <a href="https://vizwiz.org/tasks-and-datasets/vqa/">images and annotation</a>
RefCOCO | <a href="https://bvisionweb1.cs.unc.edu/licheng/referit/data/refcoco.zip"> annotations </a>
RefCOCO+ | <a href="https://bvisionweb1.cs.unc.edu/licheng/referit/data/refcoco+.zip"> annotations </a>
RefCOCOg | <a href="https://bvisionweb1.cs.unc.edu/licheng/referit/data/refcocog.zip"> annotations </a>

### Evaluation dataset structure

```
${MINIGPTv2_EVALUATION_DATASET}
├── gqa
│   └── test_balanced_questions.json
│   ├── testdev_balanced_questions.json
│   ├── gqa_images
├── hateful_meme
│   └── hm_images
│   ├── dev.jsonl
├── iconvqa
│   └── iconvqa_images
│   ├── choose_text_val.json
├── vizwiz
│   └── vizwiz_images
│   ├── val.json
├── vsr
│   └── vsr_images
├── okvqa
│   ├── okvqa_test_split.json
│   ├── mscoco_val2014_annotations_clean.json
│   ├── OpenEnded_mscoco_val2014_questions_clean.json
├── refcoco
│   └── instances.json
│   ├── refs(google).p
│   ├── refs(unc).p
├── refcoco+
│   └── instances.json
│   ├── refs(unc).p
├── refercocog
│   └── instances.json
│   ├── refs(google).p
│   ├── refs(und).p
...
```


### environment setup

```
export PYTHONPATH=$PYTHONPATH:/path/to/directory/of/MiniGPT-4
```

### start evalauting RefCOCO, RefCOCO+, RefCOCOg
port=port_number  
cfg_path=/path/to/eval_configs/minigptv2_eval.yaml  
save_path=/path/to/save/path  
ckpt=/path/to/evaluation/checkpoint  
split=data_evaluation_split  
dataset=dataset_name  

dataset | split
--- | :---:
refcoco | val, testA, testB
refcoco+ | val, testA, testB
refcocog | val, test

```
torchrun --master-port ${port} --nproc_per_node 1 eval_ref.py \
 --cfg-path ${cfg_path} --eval_file_path ${eval_file_path} --save_path ${save_path} \
 --ckpt ${ckpt} --split ${split}  --dataset ${dataset} --lora_r 64 --lora_alpha 16 \
 --batch_size 10 --max_new_tokens 20 --resample
```


### start evaluating visual question answering

port=port_number  
cfg_path=/path/to/eval_configs/minigptv2_eval.yaml  
eval_file_path=/path/to/eval/annotation/path  
image_path=/path/to/eval/image/path  
save_path=/path/to/save/path  
ckpt=/path/to/evaluation/checkpoint  
split=evaluation_data_split
dataset=dataset_type 


dataset | image_path | eval_file_path
--- | :---:| :---:
okvqa | coco_2017 | /path/to/okvqa/folder
vizwiz | vizwiz_images | /path/to/vizwiz/folder
iconvqa | iconvqa_images | /path/to/iconvqa/folder
gqa | gqa_images | /path/to/gqa/folder
vsr |  vsr_images | None
hateful meme | hm_images | /path/to/hateful_mem/folder


```
torchrun --master-port ${port} --nproc_per_node 1 eval_vqa.py \
 --cfg-path ${cfg_path} --img_path ${image_path} --eval_file_path ${eval_file_path} --save_path ${save_path} \
 --ckpt ${ckpt} --split ${split}  --dataset ${dataset} --lora_r 64 --lora_alpha 16 \
 --batch_size 10 --max_new_tokens 20 --resample
```




