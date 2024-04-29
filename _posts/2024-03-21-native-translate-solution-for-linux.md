---
title: native translate solution for linux 
date: 2024-03-21
---

# Linux 平台本地翻译解决方案

离线翻译工具：

[argos-translate](https://github.com/argosopentech/argos-translate)

```sh
# install argos
pip install argostranslate
pip install argostranslategui

argospm search
argospm install translate-en_zh
argospm install translate-zh_en
argos-translate --from en --to zh "hello"
```

text to speech：tts

```sh
pip install tts
pip-server --list_models
pip-server --model_name <model_name>
# e.g.
tts-server --model_name tts_models/en/vctk/vits
```



