# FerrumPix Models

This repository is a place to keep files. The weights and the source data come
**from other projects**, not from here: the models were converted to ONNX so that
FerrumPix can run them without Python, and the place table was built from the
GeoNames dataset. Those conversions are the only work done here — no model was
trained, no weight was altered. Each file stays under the licence of the project
it came from.

## Licences

**There is no single licence covering this repository.** Every file carries the
licence of the project it came from:

| File | Origin | Licence | Notices |
|---|---|---|---|
| `mobilesam-encoder-v1.onnx` | [MobileSAM](https://github.com/ChaoningZhang/MobileSAM) | Apache-2.0 | `licenses/LICENSE.Apache-2.0.txt`, `licenses/NOTICE-mobilesam.txt` |
| `mobilesam-decoder-v1.onnx` | [MobileSAM](https://github.com/ChaoningZhang/MobileSAM) | Apache-2.0 | `licenses/LICENSE.Apache-2.0.txt`, `licenses/NOTICE-mobilesam.txt` |
| `midas-small-v1.onnx` | [MiDaS](https://github.com/isl-org/MiDaS) | MIT | `licenses/LICENSE.MIT.midas.txt`, `licenses/NOTICE-midas.txt` |
| `lama-v1.onnx` | [LaMa](https://github.com/advimman/lama) | Apache-2.0 | `licenses/LICENSE.Apache-2.0.txt`, `licenses/NOTICE-lama.txt` |
| `scunet-v1.onnx` | [SCUNet](https://github.com/cszn/SCUNet) | Apache-2.0 | `licenses/LICENSE.Apache-2.0.txt`, `licenses/NOTICE-scunet.txt` |
| `nafnet-v1.onnx` | [NAFNet](https://github.com/megvii-research/NAFNet) | MIT | `licenses/LICENSE.MIT.nafnet.txt`, `licenses/NOTICE-nafnet.txt` |
| `realesrgan-x4-v1.onnx` | [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) | BSD-3-Clause | `licenses/LICENSE.BSD-3-Clause.realesrgan.txt`, `licenses/NOTICE-realesrgan.txt` |
| `realesrgan-x2-v1.onnx` | Real-ESRGAN | BSD-3-Clause | `licenses/LICENSE.BSD-3-Clause.realesrgan.txt`, `licenses/NOTICE-realesrgan.txt` |
| `realesrgan-fast-x4-v1.onnx` | Real-ESRGAN | BSD-3-Clause | `licenses/LICENSE.BSD-3-Clause.realesrgan.txt`, `licenses/NOTICE-realesrgan.txt` |
| `realesrgan-fast-wdn-x4-v1.onnx` | Real-ESRGAN | BSD-3-Clause | `licenses/LICENSE.BSD-3-Clause.realesrgan.txt`, `licenses/NOTICE-realesrgan.txt` |
| `realesrgan-anime-x4-v1.onnx` | Real-ESRGAN | BSD-3-Clause | `licenses/LICENSE.BSD-3-Clause.realesrgan.txt`, `licenses/NOTICE-realesrgan.txt` |
| `yunet-v1.onnx` | [OpenCV Model Zoo](https://github.com/opencv/opencv_zoo) | MIT | `licenses/LICENSE.MIT.yunet.txt`, `licenses/NOTICE-yunet.txt` |
| `orte-v1.sqlite` | [GeoNames](https://www.geonames.org/) | CC BY 4.0 | `licenses/LICENSE.CC-BY-4.0.txt`, `licenses/NOTICE-orte.txt` |

## ArcFace is *not* in this collection

`arcface-r100-v1.onnx`, the model that compares faces, is **not** part of these releases.
FerrumPix fetches it from its publisher instead:

    https://huggingface.co/onnxmodelzoo/arcfaceresnet100-8

The reason is its licence. The ONNX Model Zoo publishes the file under the Apache-2.0 of its own
repository, but the weights come from [InsightFace](https://github.com/deepinsight/insightface),
and there the pretrained models are limited to **non-commercial research purposes only** — the
MIT licence of that project covers its code, not its weights. Redistributing the file would pass
on a restriction that a GPL-3 application cannot pass on, because the GPL guarantees every
recipient the right to use the program commercially. Fetching it from the publisher leaves that
question where it belongs: between the user and InsightFace.

`licenses/NOTICE-arcface.txt` stays in this repository as the record of where the file comes from.

YuNet, which *finds* the faces, is unaffected: it is MIT from the OpenCV Model Zoo and grew out of
[libfacedetection](https://github.com/ShiqiYu/libfacedetection). The two are not interchangeable —
YuNet finds faces and the five points the alignment stands on, ArcFace compares an already aligned
crop.

MobileSAM builds on Segment Anything by Meta Platforms and on TinyViT by
Microsoft. Both copyright notices are reproduced in the notice file.

LaMa's weights are the "big-lama" checkpoint the upstream README names as its own
download. The notice file records the full chain: original project, weights, and
the ONNX export this copy came from.

The two denoising files are not two versions of one thing. They do the same job
at different prices: SCUNet keeps more of the faint drawing in a picture, NAFNet
runs about six times faster and is a little smoother. Turning one down does not
turn it into the other, which is why both are here rather than one. Their notice
files carry the measured comparison.

The five enlargers are not versions of one thing either. Each was trained on a
different kind of picture, and that is what you see in the result. Which one
suits a photo is a matter of looking, not of a number - that is why they are all
here.

The CC BY licence on the place table asks for something the others do not:
whoever passes that file on names GeoNames as its source.

Every licence in this collection permits redistribution — which is exactly why
ArcFace is *not* in it. Whoever passes one of these files on
has to pass on its licence text and its copyright notices with it - CC BY
additionally requires the source to be named. That is why the `licenses` folder
belongs to every release, not just to the source tree.

## What was changed

The weights were exported to ONNX unchanged, without retraining and without
altering anything about the weights themselves. Each notice file records how its
file was produced, and where.

The place table was converted from the GeoNames text dumps into SQLite and cut
down to the fields needed for the lookup. No place was added, removed or altered.

`lama-v1.onnx` is a half-precision export: only the convolution weights are
stored as float16, with a cast in front of each. Input and output stay float32
and the arithmetic is done in float32, so nothing has to be converted by the
caller.

`scunet-v1.onnx` came as two files - a small graph and a separate weight blob -
and the weights were folded back into the graph file. Nothing else about it was
touched. It is one file here because FerrumPix verifies one file per model
against one checksum, which is also why its SHA-256 differs from the source.

`nafnet-v1.onnx` was exported to ONNX for FerrumPix from the upstream
NAFNet-SIDD-width32 checkpoint, without retraining and without altering a
weight.

The five `realesrgan-*` files were exported to ONNX here from the author's own
published weights, without retraining and without altering a weight. Each export
was compared against the original: the difference is floating point rounding.

Apache-2.0 requires changes to be stated. Converting the file format is one, so
it is stated here and in the notice files.

## Checksums

`SHA256SUMS.txt` accompanies every release. FerrumPix carries the same checksums
**inside the application** and verifies every file it fetches against them. A
checksum that travelled with the file would be supplied by the same attacker who
supplied the file.

## Installing by hand

The usual way is the button in the FerrumPix settings, under Models. Anyone
who would rather place the files themselves - on a machine without a network
connection, for instance - puts them here:

| System | Folder |
|---|---|
| Linux | `~/.config/FerrumPix/Modelle` |
| Windows | `%APPDATA%\FerrumPix\Modelle` |
| macOS | `~/Library/Application Support/FerrumPix/Modelle` |

A `Modelle` folder next to the application and `/usr/share/ferrumpix/modelle` are
searched as well. Whatever sits in the user's own folder wins.

The file names have to stay as they are - FerrumPix looks for them by name and
checks their checksum.

## What is needed for what

| Feature | Files | Together |
|---|---|---|
| Select an object by clicking it (mask tool) | `mobilesam-encoder-v1.onnx`, `mobilesam-decoder-v1.onnx` | 42 MiB |
| Depth mask and depth blur | `midas-small-v1.onnx` | 63 MiB |
| Remove an object | `lama-v1.onnx` | 105 MiB |
| Denoise a photo, thorough | `scunet-v1.onnx` | 73 MiB |
| Denoise a photo, quick | `nafnet-v1.onnx` | 113 MiB |
| Enlarge four times, thorough | `realesrgan-x4-v1.onnx` | 64 MiB |
| Enlarge twice, thorough | `realesrgan-x2-v1.onnx` | 64 MiB |
| Enlarge four times, quick | `realesrgan-fast-x4-v1.onnx` | 4.6 MiB |
| Enlarge four times, quick, keeping the grain | `realesrgan-fast-wdn-x4-v1.onnx` | 4.6 MiB |
| Enlarge a drawing | `realesrgan-anime-x4-v1.onnx` | 17 MiB |
| Find people and group them | `yunet-v1.onnx` from here, plus `arcface-r100-v1.onnx` from its publisher | 249 MiB |
| Name the place a photo was taken | `orte-v1.sqlite` | 12 MiB |

They are independent of one another: if you only want the object selection, fetch
only its two files. When a piece is missing, the matching controls in FerrumPix are not
there - not greyed out, gone.

The enlargers are independent of each other too, and one of them is enough to
start. FerrumPix offers exactly those you have. If you are unsure which, take
*four times, thorough* for photographs; without a graphics card take *four times,
quick*, which is fourteen times smaller and much faster and still puts back more
than a plain enlargement does.

In the FerrumPix settings, denoising and enlarging each stand under one entry,
and the button there fetches everything belonging to it - all five enlargers are
154 MiB. Anyone who wants a single one of them takes it from the release page and
puts it in the folder below by hand. FerrumPix picks it up either way and says
how many of the files it has found.

The two people files only work as a pair. YuNet finds the faces - where they are,
plus the five points the alignment rests on - and ArcFace turns one aligned face
into a row of 512 numbers that can be compared. ArcFace cannot search: without
YuNet there would be nothing to hand it. With YuNet alone you would know that
somebody is in the picture, never whether it is the same person as next door.
Neither of them knows who anyone is - names are yours to give.

The size is lopsided on purpose: finding faces costs 227 KiB, telling them apart
costs 249 MiB. That is where the difficulty sits.

`orte-v1.sqlite` is the odd one out: not a learned model but a lookup table of
170540 places with their coordinates, derived from the GeoNames dataset. It
answers a single question - which place is nearest to this point - and it answers
it locally, so photo positions never leave the machine. It knows places, not
streets or addresses, and a photo out at sea deliberately gets no name: the
nearest entry would be a thousand kilometres away.

Everything runs on your own machine. Nothing is transmitted to anyone, and
FerrumPix never fetches anything by itself.

## Versions

The version is part of the file name. A file that already sits on the machine
stays valid and FerrumPix keeps working with it until someone asks for the
update.

What matters for that is the **contract** - the inputs, their shapes and their
value ranges. Where a file keeps its name, the checksum is what tells one build
apart from another, and FerrumPix checks it: a copy that does not match the
checksum in the application is fetched again rather than used. Where the contract
itself changes, the file gets a new name. Faces are the one place where a change
is felt in the library as well: the rows of numbers only ever compare against rows
from the same model, so a library scanned with a different one has to be scanned
once more.
