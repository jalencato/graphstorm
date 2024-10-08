# Inference script examples for link prediction
This folder provides example yaml configurations for link prediction inference tasks. The configurations include:

  * ``ml_lp_infer.yaml`` defines a link prediction task on the ``(user, rating, movie)`` edges. It uses a single-layer RGCN model as its graph encoder and uses a dot product decoder.

  * ``ml_lp_text_infer.yaml`` defines a link prediction task on the ``(user, rating, movie)`` edges. It uses a single-layer RGCN model as its graph encoder and uses a dot product decoder. The input node features of ``movie`` nodes and ``user`` nodes will be produced by a BERT model during inference.

  * ``ml_lm_lp_infer.yaml`` defines a link prediction task on the ``(user, rating, movie)`` edges. It uses a language model, i.e., the BERT model, as its graph encoder and uses a dot product decoder. The node features of ``movie`` nodes and ``user`` nodes are produced by the BERT model during inference.

  * ``ml_lm_input_lp_infer.yaml``defines a link prediction task on the ``(user, rating, movie)`` edges. It uses a language model, i.e., the BERT model, plus an MLP layer as its graph encoder, and uses a dot product decoder. The node features of ``movie`` nodes and ``user`` nodes are produced by the BERT model during inference.

You can find the corresponding training configurations in ``training_scripts/gsgnn_lp/README``.

The following example script shows how to launch a GraphStorm link prediction inference task.
You may need to change the arguments according to your tasks.
For more detials please refer to https://graphstorm.readthedocs.io/en/latest/cli/model-training-inference/index.html.

```
python3 -m graphstorm.run.gs_link_prediction \
    --inference \
    --workspace graphstorm/inference_scripts/lp_infer/ \
    --num-trainers 4 \
    --num-servers 1 \
    --num-samplers 0 \
    --part-config /data/movielen_100k_lp_train_val_1p_4t/movie-lens-100k.json \
    --ip-config ip_list.txt \
    --ssh-port 2222 \
    --cf ml_lp_infer.yaml \
    --restore-model-path /data/gsgnn_lp/epoch-2/ \
    --save-embed-path /data/gsgnn_lp_ml_dot/infer-emb
```

The script loads a paritioned graph from ``/data/movielen_100k_lp_train_val_1p_4t/movie-lens-100k.json``
and a pre-trained model from ``/data/gsgnn_lp/epoch-2/`` to do link prediction inference.
The script loads all the inference task specific configurations from ``ml_lp_infer.yaml``.
The output embeddings are saved in ``/data/gsgnn_lp_ml_dot/infer-emb``.

Note: All example movielens graph data are generated by ``tests/end2end-tests/create_data.sh``.
