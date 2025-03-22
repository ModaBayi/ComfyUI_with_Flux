FROM valyriantech/comfyui-without-flux:latest

# Modellerin konacağı klasörleri önceden oluştur
RUN mkdir -p /workspace/ComfyUI/models/unet \
    && mkdir -p /workspace/ComfyUI/models/clip \
    && mkdir -p /workspace/ComfyUI/models/vae

# Modelleri Huggingface'ten indir
RUN wget --header="Authorization: Bearer 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" \
    -O /workspace/ComfyUI/models/unet/flux1-dev.safetensors \
    "https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/flux1-dev.safetensors"

RUN wget --header="Authorization: Bearer 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" \
    -O /workspace/ComfyUI/models/unet/flux1-fill-dev.safetensors \
    "https://huggingface.co/black-forest-labs/FLUX.1-Fill-dev/resolve/main/flux1-fill-dev.safetensors"

RUN wget -O /workspace/ComfyUI/models/clip/clip_l.safetensors \
    "https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/clip_l.safetensors"

RUN wget --header="Authorization: Bearer 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" \
    -O /workspace/ComfyUI/models/clip/t5xxl_fp16.safetensors \
    "https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/t5xxl_fp16.safetensors"

RUN wget --header="Authorization: Bearer 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" \
    -O /workspace/ComfyUI/models/vae/ae.safetensors \
    "https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/ae.safetensors"

RUN wget --header="Authorization: Bearer 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" \
    -O /workspace/ComfyUI/models/vae/diffusion_pytorch_model.safetensors \
    "https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/vae/diffusion_pytorch_model.safetensors"

RUN wget -O /workspace/ComfyUI/models/clip/EVA02_CLIP_L_336_psz14_s6B.pt \
    "https://huggingface.co/QuanSun/EVA-CLIP/resolve/main/EVA02_CLIP_L_336_psz14_s6B.pt"

# Download and move clip_l.safetensors
RUN wget -O /ComfyUI/models/clip/clip_l.safetensors "https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/clip_l.safetensors?download=true" --progress=bar:force:noscroll

# Download and move t5xxl_fp8_e4m3fn.safetensors
RUN wget -O /ComfyUI/models/clip/t5xxl_fp8_e4m3fn.safetensors "https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/t5xxl_fp8_e4m3fn.safetensors?download=true" --progress=bar:force:noscroll

# Download LoRas
RUN wget -O /ComfyUI/models/loras/GracePenelopeTargaryenV5.safetensors "https://huggingface.co/WouterGlorieux/GracePenelopeTargaryenV5/resolve/main/GracePenelopeTargaryenV5.safetensors?download=true" --progress=bar:force:noscroll
RUN wget -O /ComfyUI/models/loras/VideoAditor_flux_realism_lora.safetensors "https://huggingface.co/VideoAditor/Flux-Lora-Realism/resolve/main/flux_realism_lora.safetensors?download=true" --progress=bar:force:noscroll

# make the xlabs directory
RUN mkdir -p /ComfyUI/models/xlabs/loras
RUN wget -O /ComfyUI/models/xlabs/loras/Xlabs-AI_flux-RealismLora.safetensors "https://huggingface.co/XLabs-AI/flux-RealismLora/resolve/main/lora.safetensors?download=true" --progress=bar:force:noscroll

# This step is optional, the reActor node will download these models automatically on first launch if they are not present
# To include then in the docker image we need to download them manually and copy them to a folder called facerestore_models
# I'm not sure where to download them from, I just copied them from and existing installation
# Check if the directory exists and copy its contents if it does
COPY facerestore_models /ComfyUI/models/facerestore_models/

# make the directory for the openpose models and download the models needed
RUN mkdir -p /ComfyUI/custom_nodes/comfyui_controlnet_aux/ckpts/lllyasviel/Annotators && \
    wget "https://huggingface.co/lllyasviel/Annotators/resolve/main/body_pose_model.pth?download=true" -O /ComfyUI/custom_nodes/comfyui_controlnet_aux/ckpts/lllyasviel/Annotators/body_pose_model.pth && \
    wget "https://huggingface.co/lllyasviel/Annotators/resolve/main/hand_pose_model.pth?download=true" -O /ComfyUI/custom_nodes/comfyui_controlnet_aux/ckpts/lllyasviel/Annotators/hand_pose_model.pth && \
    wget "https://huggingface.co/lllyasviel/Annotators/resolve/main/facenet.pth?download=true" -O /ComfyUI/custom_nodes/comfyui_controlnet_aux/ckpts/lllyasviel/Annotators/facenet.pth
# Custom nodes installation
RUN mkdir -p /workspace/ComfyUI/custom_nodes

RUN git clone https://github.com/lldacing/ComfyUI_PuLID_Flux_ll.git /workspace/ComfyUI/custom_nodes/ComfyUI_PuLID_Flux_ll \
    && pip install -r /workspace/ComfyUI/custom_nodes/ComfyUI_PuLID_Flux_ll/requirements.txt

RUN git clone https://github.com/sipie800/ComfyUI-PuLID-Flux-Enhanced.git /workspace/ComfyUI/custom_nodes/ComfyUI-PuLID-Flux-Enhanced

RUN git clone https://github.com/ltdrdata/ComfyUI-Impact-Pack.git /workspace/ComfyUI/custom_nodes/ComfyUI-Impact-Pack \
    && pip install -r /workspace/ComfyUI/custom_nodes/ComfyUI-Impact-Pack/requirements.txt

RUN git clone https://github.com/ltdrdata/ComfyUI-Impact-Subpack.git /workspace/ComfyUI/custom_nodes/ComfyUI-Impact-Subpack \
    && pip install -r /workspace/ComfyUI/custom_nodes/ComfyUI-Impact-Subpack/requirements.txt

RUN git clone https://github.com/lldacing/ComfyUI_Patches_ll.git /workspace/ComfyUI/custom_nodes/ComfyUI_Patches_ll

RUN git clone https://github.com/rgthree/rgthree-comfy.git /workspace/ComfyUI/custom_nodes/rgthree-comfy

RUN git clone https://github.com/Suzie1/ComfyUI_Comfyroll_CustomNodes.git /workspace/ComfyUI/custom_nodes/ComfyUI_Comfyroll_CustomNodes

RUN git clone https://github.com/cubiq/ComfyUI_essentials.git /workspace/ComfyUI/custom_nodes/ComfyUI_essentials \
    && pip install -r /workspace/ComfyUI/custom_nodes/ComfyUI_essentials/requirements.txt

RUN git clone https://github.com/chengzeyi/Comfy-WaveSpeed.git /workspace/ComfyUI/custom_nodes/Comfy-WaveSpeed

RUN git clone https://github.com/kijai/ComfyUI-KJNodes.git /workspace/ComfyUI/custom_nodes/ComfyUI-KJNodes \
    && pip install -r /workspace/ComfyUI/custom_nodes/ComfyUI-KJNodes/requirements.txt

RUN git clone https://github.com/pythongosssss/ComfyUI-Custom-Scripts.git /workspace/ComfyUI/custom_nodes/ComfyUI-Custom-Scripts

RUN git clone https://github.com/JPS-GER/ComfyUI_JPS-Nodes.git /workspace/ComfyUI/custom_nodes/ComfyUI_JPS-Nodes

RUN git clone https://github.com/lquesada/ComfyUI-Inpaint-CropAndStitch.git /workspace/ComfyUI/custom_nodes/ComfyUI-Inpaint-CropAndStitch

CMD [ "/start.sh" ]
