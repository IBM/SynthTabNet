# SynthTabNet

SynthTabNet is a dataset of 600k `png` images from synthetically generated table layouts with annotations in `jsonl` files.


## Overview

SynthTabNet is a synthetically generated dataset that contains annotated images of data in tabular layouts.

It [has been shown](https://arxiv.org/abs/2203.01017) that other non-synthetic datasets like [PubTabNet](https://developer.ibm.com/exchanges/data/all/pubtabnet/), [FinTabNet](https://developer.ibm.com/exchanges/data/all/fintabnet/) and [TableBank](https://doc-analysis.github.io/tablebank-page/index.html) suffer from many limitations:

- Their table distributions are skewed towards simpler structures with fewer number of rows/columns.
- There is a very limited variance in the appearance styles.
- The content is sometimes restricted to certain domains.
- The bounding boxes are omitted for non-empty cells or they are completely absent.

SynthTabNet aims to overcome these limitations by providing:

- A broad range of table sizes and richer combinations of row spans /column spans.
- A variety of domain specific styling appearances (e.g. financial data, marketing data, sparse tables etc.)
- Content generated out of the most frequently used terms appearing in non-synthetic datasets (e.g. PubTabNet, FinTabNet, etc.)
- Bounding boxes for all table cells, including the empty ones.
- Rectangular table structures. For each table, every row has the same number of columns after taking into account any row spans /column spans.

SynthTabNet is organized into 4 parts of 150k tables (600k in total). Each part contains tables with different appearances in regard to their size, structure, style and content. All parts are divided into Train, Test and Val splits (80%, 10%, 10%). The tables are delivered as `png` images and the annotations are in `jsonl` format.

A detailed description of the data synthesis process can be found in the [paper](https://arxiv.org/abs/2203.01017).


## Download

v2.0.0

| Appearance style | Records | Size(GB) | URL v2.0.0  |
|------------------|---------|----------|-------------|
| Fintabnet        | 150k    | 10     | [SynthTabNet-part1](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/fintabnet.zip) |
| Marketing        | 150k    | 8      | [SynthTabNet-part2](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/marketing.zip) |
| PubTabNet        | 150k    | 6      | [SynthTabNet-part3](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/pubtabnet.zip) |
| Sparse           | 150k    | 3      | [SynthTatNet-part4](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/pubtabnet.zip) |

[v2.0.0 MD5 checksums](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/md5sum.txt)

[v2.0.0 SHA1 checksums](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v2.0.0/sha1sum.txt)


<details>
<summary>v1.0.0</summary>

| Appearance style | Records | Size(GB) | URL v1.0.0  |
|------------------|---------|----------|-------------|
| Fintabnet        | 150k    | 10     | [SynthTabNet-part1](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/fintabnet.zip) |
| Marketing        | 150k    | 8      | [SynthTabNet-part2](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/marketing.zip) |
| PubTabNet        | 150k    | 6      | [SynthTabNet-part3](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/pubtabnet.zip) |
| Sparse           | 150k    | 3      | [SynthTatNet-part4](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/sparse.zip) |

[v1.0.0 MD5 checksums](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/md5sum.txt)

[v1.0.0 SHA1 checksums](https://ds4sd-public-artifacts.s3.eu-de.cloud-object-storage.appdomain.cloud/datasets/synthtabnet_public/v1.0.0/sha1sum.txt)


</details>

## Data format

Each part of the dataset corresponds to a top level directory (`fintabnet`, `marketing`, `pubtabnet`, `sparse`) and has the following structure:

```
├── images
│   ├── test
│   ├── train
│   └── val
├── synthetic_data.jsonl
```

The annotations for each part are in the `synthetic_data.jsonl` file. Each line is a `json` object that corresponds to a `png` image and has the following structure:

```
"filename": "png image filename inside one of the 'test', 'train', 'val' directories",
"split": "One of 'test', 'train', 'val'",
"html": "Table structure and content",
    "cells": "Array with all table cells",
        "cell_id": "Zero based cell counter",
        "is_header": "true if that cell is part of the table header",
        "span": "In case there is a rowspan / columnspan",
            "spantype": "One of 'rowspan', 'colspan', '2dspan'. The '2dspan' is used in case there is a rowspan and colspan in the same cell",
            "rowspan": "Number of rowspans for this cell",
            "colspan": "Number of colspans for this cell"
        "tokens": "Array with the tokenized content of the cell",
        "bbox": "The bounding bbox and the class of the cell in [x1, y1, x2, y2, class] format"
    "structure":
        "tokens": "Array with html tags that describe the table structure"
```

Regarding the `bbox` parameter notice that:

- The coordinates origin is the top left corner of the image.
- Each bbox is described by its top left corner `(x1, y1)` and bottom right corner `(x2, y2)`.
- The bbox `class` can have the values:
  - `1`: An empty cell
  - `2`: A non-empty cell

The `tokens` can be one of:

```
" colspan=\"10\"", " colspan=\"2\"", " colspan=\"3\"", " colspan=\"4\"", " colspan=\"5\"",
" colspan=\"6\"", " colspan=\"7\"", " colspan=\"8\"", " colspan=\"9\"", " rowspan=\"10\"",
" rowspan=\"2\"", " rowspan=\"3\"", " rowspan=\"4\"", " rowspan=\"5\"", " rowspan=\"6\"",
" rowspan=\"7\"", " rowspan=\"8\"", " rowspan=\"9\"", "</tbody>", "</td>", "</thead>",
"</tr>", "<end>", "<pad>", "<start>", "<tbody>", "<td", "<td>", "<thead>", "<tr>", "<unk>", ">"
```


## Example data

![pubtabnet](pics/image_000005_1634629104.274936.png)

![sparse](pics/image_000005_1634629370.551275.png)

![fintabnet](pics/image_000014_1634629328.541362.png)

![marketing](pics/image_000024_1634629424.186544.png)


## Jupyter notebook

Here is a jupyter notebook that demonstrates how to download and use the dataset:

[Demo Notebook](synthtabnet_demo.ipynb)


## Paper

**"TableFormer: Table Structure Understanding with Transformers"** (CVPR 2022).
- Ahmed Nassar (ahn@zurich.ibm.com)
- Nikolaos Livathinos (nli@zurich.ibm.com)
- Maksym Lysak (mly@zurich.ibm.com)
- Peter Staar (taa@zurich.ibm.com)

ArXiv link: https://arxiv.org/abs/2203.01017

**Citation:**

```
@article{nassar2022tableformer,
  title={TableFormer: Table Structure Understanding with Transformers},
  author={Nassar, Ahmed and Livathinos, Nikolaos and Lysak, Maksym and Staar, Peter},
  journal={arXiv preprint arXiv:2203.01017},
  year={2022}
}
```
