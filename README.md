# FerrumPix Models

This repository is a place to keep files. It contains **no original work** of its
own, only model files from other projects, converted to ONNX so that FerrumPix
can run them without Python.

## Licences

**There is no single licence covering this repository.** Every file carries the
licence of the project it came from:

| File | Origin | Licence | Notices |
|---|---|---|---|
| `mobilesam-encoder-v1.onnx` | [MobileSAM](https://github.com/ChaoningZhang/MobileSAM) | Apache-2.0 | `licenses/LICENSE.Apache-2.0.txt`, `licenses/NOTICE-mobilesam.txt` |
| `mobilesam-decoder-v1.onnx` | [MobileSAM](https://github.com/ChaoningZhang/MobileSAM) | Apache-2.0 | the same two |
| `midas-small-v1.onnx` | [MiDaS](https://github.com/isl-org/MiDaS) | MIT | `licenses/LICENSE.MIT.midas.txt`, `licenses/NOTICE-midas.txt` |

MobileSAM builds on Segment Anything by Meta Platforms and on TinyViT by
Microsoft. Both copyright notices are reproduced in the notice file.

Both licences permit redistribution. Whoever passes one of these files on has to
pass on its licence text and its copyright notices with it. That is why the
`licenses` folder belongs to every release, not just to the source tree.

## What was changed

The weights were exported to ONNX unchanged (`torch.onnx.export`, opset 17, no
quantisation). Nothing was retrained and nothing about the weights was altered.
Each export was measured against the original; the deviations are recorded in the
notice files.

Apache-2.0 requires changes to be stated. Converting the file format is one, so
it is stated here and in the notice files.

## Checksums

`SHA256SUMS.txt` accompanies every release. FerrumPix carries the same checksums
**inside the application** and verifies every file it fetches against them. A
checksum that travelled with the file would be supplied by the same attacker who
supplied the file.

## Installing by hand

The usual way is the button in the FerrumPix settings, under Technology. Anyone
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

The two are independent: if you only want the object selection, fetch only its
two files. When a piece is missing, the matching controls in FerrumPix are not
there - not greyed out, gone.

Everything runs on your own machine. Nothing is transmitted to anyone, and
FerrumPix never fetches anything by itself.

## Versions

The version is part of the file name. A new model gets a new name; the old file
stays valid and FerrumPix keeps working with it until someone asks for the
update.
