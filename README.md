#  Stable Diffusion Sketch [![Version](https://img.shields.io/badge/Version-0.12.5-blue)](https://github.com/jordenyt/stable_diffusion_sketch/releases/latest)
Stable Diffusion Sketch 是一款可以让您在 Android 平台使用 [Automatic1111's Stable Diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)的应用 **你自己的服务器**. <br/>

**[下载汉化修改版 APK](https://github.com/jordenyt/stable_diffusion_sketch/releases/latest)**

## 支持的功能

- Support ControlNet v1.1（支持 ControlNet v1.1）
- Support SDXL（支持 SDXL）
- Autocomplete LORA tag in prompt（在提示中自动完成 LORA 标签）
- Sketch with color（带颜色的草图）
- Create new paint from（从以下选项创建新绘画）:
  - Blank Canvas（空白画布）
  - Capture from camera（从相机拍摄）
  - Output of Stable Diffusion txt2img（稳定扩散 txt2img 的输出）
  - shared image from other apps（从其他应用程序中共享的图像）
- Enhance your sketch with Stable Diffusion（使用稳定扩散增强您的草图）
  - Preset Modes（预设模式）:
    - img2img(sketch) + Scribble(sketch)（img2img（图像到图像）（草图）+ 涂鸦（草图））
    - img2img(sketch) + Depth(sketch)（img2img（图像到图像）（草图）+ 深度（草图））
    - img2img(sketch) + Pose(sketch)（img2img（图像到图像）（草图）+ 姿势（草图））
    - txt2img + Canny(sketch)（txt2img + Canny（草图））
    - txt2img + Scribble(sketch)（txt2img + 涂鸦（草图））
    - txt2img + Depth(sketch)（txt2img + 深度（草图））
    - txt2img + Pose(sketch)（txt2img + 姿势（草图））
    - Inpainting (background)（修补绘制背景）
    - Inpainting (sketch)（修补绘制草图）
    - Partial Inpainting (background)（局部修补绘制背景）
  - Special Modes（特殊模式）:
    - Outpainting（外部绘制）
    - Fill with Reference（用参考图填充）
    - Merge with Reference（与参考图合并）
    - SDXL Refiner（SDXL 优化器）
  - 8 Custom Modes（8 自定义模式）
- Painting Tools（绘画工具）:
  - Palette（调色板）
  - Paintbrush（画笔）
  - Eyedropper（吸管工具）
  - Eraser（橡皮擦）
  - Undo/redo（撤销/重做）
- Preset values for your prompt（提示的预设值）
  - Prompt Prefix（提示前缀）
  - Prompt Postfix（提示后缀）
  - Negative Prompt（负面提示）
- 3 Canvas aspect ratio（3 种画布纵横比）: landscape（横向）, portrait（纵向）, and square（正方形）
- Upscaler（图像增强工具）
- Long press image on Main Screen to delete（在主屏幕上长按图像以删除）
- Group related sketches（将相关的草图分组）
- Support multiple ControlNet（支持多个 ControlNet）
- Keep EXIF of shared content in your SD output（在 SD 输出中保留共享内容的 EXIF 信息）

## Custom Modes（自定义模式）
Custom mode can be defined in JSON format.（可以用JSON格式定义自定义模式）<br/>

### Examples
1. Partial inpaint with POSE <br/>
`{"type":"inpaint","denoise":0.75, "baseImage":"background", "inpaintFill":1, "inpaintPartial":1, "cn":[{"cnInputImage":"background", "cnModelKey":"cnPoseModel", "cnModule":"openpose_full", "cnWeight":1.0, "cnControlMode":0}], "sdSize":768}`
2. Color fix <br/>
`{"type":"inpaint","denoise":0.5, "baseImage":"background", "inpaintFill":1, "inpaintPartial":1, "cn":[{"cnInputImage":"background", "cnModelKey":"cnSoftedgeModel", "cnModule":"softedge_pidinet", "cnWeight":1.0, "cnControlMode":0}], "sdSize":1024}`
3. Mild Enhance <br/>
`{"type":"inpaint","denoise":0.15, "baseImage":"background", "inpaintFill":1, "inpaintPartial":1, "cn":[{"cnInputImage":"background", "cnModelKey":"cnTileModel", "cnModule":"tile_resample", "cnModuleParamA":1, "cnWeight":1.0, "cnControlMode":0}], "sdSize":1024}`
4. Heavy Enhance <br/>
`{"type":"inpaint","denoise":0.4, "baseImage":"background", "inpaintFill":1, "inpaintPartial":1, “cn”:[{"cnInputImage":"background", "cnModelKey":"cnTileModel", "cnModule":"tile_colorfix+sharp", "cnModuleParamA":5, "cnModuleParamB":0.2, "cnWeight":1.0, "cnControlMode":0}], "sdSize":1024}`
5. Partial Redraw <br/>
`{"type":"inpaint", "denoise":0.7, "baseImage":"background", "inpaintFill":1, "inpaintPartial":1, "sdSize":1024}`
6. Get similar image <br/>
`{"type":"txt2img", "cn":[{"cnInputImage":"background", "cnModelKey":"cnNoneModel", "cnModule":"reference_only", "cnWeight":1.0, "cnControlMode":2}]}`
7. Tiles Refiner <br/>
`{"type":"img2img", "denoise":0.4, "baseImage":"background", "cn":[{ "cnInputImage":"background", "cnModelKey":"cnTileModel", "cnModule":"tile_colorfix+sharp", "cnModuleParamA":4, "cnModuleParamB":0.1, "cnWeight":1.0}], "sdSize":1024}`
8. Partial Inpaint with LORA <br/>
`{"type":"inpaint", "denoise":0.9, "cfgScale":7, "inpaintFill":1, "baseImage":"background", "sdSize":1024, "model":"v1Model"}`

### Parameters for the mode definition JSON（模式定义 JSON 的参数）:
| Variable（变量） | txt2img | img2img | inpainting | Value                                                                                                            |
|------------------|---------|---------|------------|------------------------------------------------------------------------------------------------------------------|
| `type`           | M       | M       | M          | `txt2img` - 文生图 <br /> `img2img` - 图生图 <br /> `inpaint` - 修复                        |
| `steps`          | O       | O       | O          | integer from 1 to 120, default value is 40                                                                       |
| `cfgScale`       | O       | O       | O          | decimal from 0 to 30, default value is 7.0                                                                       |
| `model`          | O       | O       | O          | `v1Model` - txt2img 和 img2img 模式的默认值 <br/> `v1Inpaint` - 默认修复 <br/> `sdxlBase` - 默认SDXL文生图模型 <br/> `sdxlRefiner` - 默认的SDXL Refiner 模式|
| `denoise`        | -       | M       | M          | decimal from 0 to 1                                                                                              |
| `baseImage`      | -       | M       | M          | `background` - 绘图下的背景图像 <br/> `sketch` - 你在背景图像上画的画         |
| `inpaintFill`    | -       | -       | O          | `0` - fill (DEFAULT) <br/> `1` - original <br/> `2` - latent noise <br/> `3` - latent nothing                    |
| `inpaintPartial` | -       | -       | O          | `0` - Inpainting on whole image (DEFAULT) <br/> `1` - Inpainting on "painted" area and paste on original image   |
| `sdSize`         | O       | O       | O          | Output resolution of SD.  Default value is configured  in setting. <br/>Suggested value: 512 / 768 / 1024 / 1280 |
| `cn`             | O       | O       | O          | JSON Array for ControlNet Object                                                                                 |

(M - Mandatory; O - Optional)

### Parameters for ControlNet Object:
| Variable         | Value                                                                                                                                                                                                                                                                                                                                                                                           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `cnInputImage`   | `background` - background image under your drawing <br/> `sketch` - your drawing and the background image <br/> `reference` - reference image                                                                                                                                                                                                                                                   |
| `cnModelKey`     | `cnTileModel` - CN Tile Model <br/> `cnPoseModel` - CN Pose Model <br/> `cnCannyModel` - CN Canny Model <br/> `cnScribbleModel` - CN Scribble Model <br/> `cnDepthModel` - CN Depth Model <br/> `cnNormalModel` - CN Normal Model <br/> `cnMlsdModel` - CN MLSD Model <br/> `cnLineartModel` - CN Line Art Model <br/> `cnSoftedgeModel` - CN Soft Edge Model <br/> `cnSegModel` - CN Seg Model <br/> `cnOther1Model` - Other CN Model 1 <br/> `cnOther2Model` - Other CN Model 2 <br/> `cnOther3Model` - Other CN Model 3 |
| `cnModule`       | CN Module that ControlNet provided.  Typical values are: `tile_resample` / `reference_only` / `openpose_full` / `canny` / `depth_midas` / `scribble_hed` <br/> For full list, please refer to the Automatic1111 web UI.                                                                                                                                                                         |
| `cnControlMode`  | `0` - Balanced (DEFAULT) <br/> `1` - My prompt is more important <br/> `2` - ControlNet is more important                                                                                                                                                                                                                                                                                       |
| `cnWeight`       | decimal from 0 to 1                                                                                                                                                                                                                                                                                                                                                                             |
| `cnModuleParamA` | First Parameter for ControlNet Module                                                                                                                                                                                                                                                                                                                                                           |
| `cnModuleParamB` | Second Parameter for ControlNet Module                                                                                                                                                                                                                                                                                                                                                          |
## Screenshots
<img src="https://github.com/jordenyt/stable_diffusion_sketch/assets/5007252/50681a65-53a9-4368-87ec-571fc773b674" height="450"> 
<img src="https://github.com/jordenyt/stable_diffusion_sketch/assets/5007252/b7d8002c-700d-4055-9be5-17c59683ae5a" height="450"> 
<img src="https://github.com/jordenyt/stable_diffusion_sketch/assets/5007252/a83524c2-f12d-498b-8643-dccddcc89088" height="450"> 

<img src="https://github.com/jordenyt/stable_diffusion_sketch/assets/5007252/7cebca43-0745-4547-8b1c-70a95a65bce5" height="450"> 

## Demo Video (on outdated version)
https://user-images.githubusercontent.com/5007252/225839650-f55a1b4b-3fa3-4181-8989-c55af844440f.mp4

## Preset Modes Demo（预设模式演示）
| Mode                               | Config                                                                                                                                                              | Demo Input                                                                                                                   | Demo Output                                                                                                                                                                                                                                               |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| img2img(sketch) + Scribble(sketch) | `{"baseImage":"sketch", "cn":[{"cnInputImage":"sketch", "cnModelKey":"cnScribbleModel", "cnModule":"none", "cnWeight":0.7}], "denoise":0.8, "type":"img2img"}`      | <img src="https://user-images.githubusercontent.com/5007252/228425856-3600d997-4c4d-4b03-9727-7d90bde2a528.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228426672-466d23e2-730e-4186-ab84-1658750099bb.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228426679-8dbc6c20-1e99-473f-9109-b96b2a29a542.png" width="140"> |
| img2img(sketch) + Depth(sketch)    | `{"baseImage":"sketch", "cn":[{"cnInputImage":"sketch", "cnModelKey":"cnDepthModel", "cnModule":"depth_leres", "cnWeight":1.0}], "denoise":0.8, "type":"img2img"}`  | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228435631-6500ea57-f8e5-453d-97ec-be8783e66453.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228435664-77d27294-df71-472c-8f9a-38957f7c7046.png" width="140"> |
| img2img(sketch) + Pose(sketch)     | `{"baseImage":"sketch", "cn":[{"cnInputImage":"sketch", "cnModelKey":"cnPoseModel", "cnModule":"openpose_full", "cnWeight":1.0}], "denoise":0.8, "type":"img2img"}` | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228436000-e6aec212-d912-42e0-821a-0df25683ee23.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228436016-9cb7f092-99b7-445f-a06a-03beedea5d7f.png" width="140"> |
| txt2img + Canny(sketch)            | `{"cn":[{"cnInputImage":"sketch", "cnModelKey":"cnCannyModel", "cnModule":"canny", "cnWeight":1.0}], "type":"txt2img"}`                                             | <img src="https://user-images.githubusercontent.com/5007252/228436349-638047c0-97e9-43b1-8c5a-9f8a95d6d256.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228436419-1252130c-166c-462b-b4ae-b1f643492a71.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228436480-9c1112ff-2517-4c26-9875-0d51ad888d7e.png" width="140"> |
| txt2img + Scribble(sketch)         | `{"cn":[{"cnInputImage":"sketch", "cnModelKey":"cnScribbleModel", "cnModule":"scribble_hed", "cnWeight":0.7}], "type":"txt2img"}`                                   | <img src="https://user-images.githubusercontent.com/5007252/228436349-638047c0-97e9-43b1-8c5a-9f8a95d6d256.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228436620-5263c004-851a-4b95-a19b-b808a8184257.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228436637-f0898e2e-d884-4a25-942c-6b3cff1a1aad.png" width="140"> |
| txt2img + Depth(sketch)            | `{"cn":[{"cnInputImage":"sketch", "cnModelKey":"cnDepthModel", "cnModule":"depth_leres", "cnWeight":1.0}], "type":"txt2img"}`                                       | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228436747-7ec3b80a-686d-47cf-a505-c0ec27230100.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228436764-d7962b77-6006-49b1-aecb-59fc3941858b.png" width="140"> |
| txt2img + Pose(sketch)             | `{"cn":[{"cnInputImage":"sketch", "cnModelKey":"cnPoseModel", "cnModule":"openpose_full", "cnWeight":1.0}], "type":"txt2img"}`                                      | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> |                                                                                                                                                                                                                                                           |
| Inpainting(background)             | `{"baseImage":"background", "denoise":1.0, "inpaintFill":2, "type":"inpaint"}`                                                                                      | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228437109-942bd67f-1c05-4004-874f-61d818314765.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228437128-467737fe-850d-44d3-8eb2-3fbd6c136895.png" width="140"> |
| Inpainting(sketch)                 | `{"baseImage":"sketch", "denoise":0.8, "inpaintFill":1, "type":"inpaint"}`                                                                                          | <img src="https://user-images.githubusercontent.com/5007252/228435585-62fbe0f0-1cdf-42b3-821e-9dc12fc29a21.png" width="140"> | <img src="https://user-images.githubusercontent.com/5007252/228437282-6acad62a-49f9-45e4-98b5-141ee02215a3.png" width="140"> <img src="https://user-images.githubusercontent.com/5007252/228437310-8cc54a11-f3fc-45aa-a846-3f080994cf1c.png" width="140"> |
| Partial Inpainting (background)    | `{"baseImage":"background", "denoise":1.0, "inpaintFill":2, "inpaintPartial":1, "type":"inpaint"}`                                                                  |                                                                                                                              |                                                                                                                                                                                                                                                           |
| Outpainting                        | `{"baseImage":"background", "denoise":1.0, "inpaintFill":2, "type":"inpaint", "cfgScale":10.0}`                                                                     |                                                                                                                              |                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                          | | | 
| Merge with Reference               | `{"baseImage":"background", "denoise":0.75, "inpaintFill":1, "type":"inpaint"}`                                                                                     |                                                                                                                              |                                                                                                                                                                                                                                                           |

## 先决条件
在使用稳定扩散草图之前，您需要在服务器上安装并设置以下内容：

1. [stable-diffusion Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) by AUTOMATIC1111
2. 在 Stable Diffusion Web UI 上安装 [sd-webui-controlnet 扩展](https://github.com/Mikubill/sd-webui-controlnet)
3. 通过编辑运行脚本 webui-user.bat 启用 API 并监听所有网络接口：
`设置 COMMANDLINE_ARGS=--api --listen`
4. 将您喜欢的 SD 模型放在 stable-diffusion-webui/models/Stable-diffusion 文件夹下。您可以从[Civitai](https://civitai.com/)中选择一项。
5. 将您首选的 ControlNet 模型放在 stable-diffusion-webui/extensions/sd-webui-controlnet/models 文件夹下。
   - 需要 Scribble、Canny、Depth、Tile 和 Pose 模型。
   - 默认支持的模型可以从[lllyasviel的ControlNet v1.1 Hugging Face卡](https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main)下载
   - ControlNet 模型需要与您的 SD 模型匹配才能使其正常工作。即，如果您的 ControlNet 模型是针对 SD1.5 构建的，则您的 SD 模型需要基于 SD1.5。



## 使用
使用稳定扩散素描的方法如下：

1. 在您的服务器上SD Web 用户界面。
2. 在您的 Android 设备上下载并安装稳定扩散 Sketch APK。
3. 打开应用程序，并在“Stable Diffusion Server Address”字段中输入您的SD的网络地址。
   - 如果您的 Android 设备和服务器都位于相同的内部网络中，您可以使用内部 IP 地址，即 192.168.xxx.xxx / 10.xxx.xxx.xxx。您可以通过在 Windows 上运行 `ipconfig /all` 或在 MacOS/Linux 上运行 `ifconfig --all` 来获取此 IP。
   - 如果您的 Android 设备位于公共互联网上，而您的服务器位于内部网络上，您需要配置路由器的NAT/防火墙和DDNS。在这种情况下，请使用互联网 IP 和翻译的端口号作为服务器地址。
   - 您可以通过在 Android 设备的网络浏览器上使用它来测试服务器地址。如果地址有效，您将在浏览器中看到 automatic1111 的 Web 用户界面正在运行。
4. 在应用程序中，选择SD模型、修复模型、采样器、升频器和控制网络模型。
5. 开始绘图，让SD发挥魔力！
## License
Stable Diffusion Sketch is licensed under the [GNU General Public License v3.0](https://github.com/jordenyt/stable_diffusion_sketch/blob/main/LICENSE).
