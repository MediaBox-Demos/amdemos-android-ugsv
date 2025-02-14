# V3.15.0

## Feature updates

1. Fixed the bug where produced videos freeze during playback.
2. Fixed the bug where the time effects applied to different segments in a video fail to take effect.
3. Fixed the bug where the front cameras of some certain devices do not support specifying exposure areas.
4. Added two groups of transition effects and two groups of filter effects based on the effect customization specifications.

## Operation changes

1. Added an operation for modifying parameters of custom effects. You can call this operation to adjust effects in real time.
2. Supported custom filter and transition effects. For more information about effect customization specifications, see the official documentation.

——————————————————————————————————————————————————————

# Effect customization

The short video SDK allows you to customize effects. You can use OpenGL ES 3.0 shaders to customize filter and transition effects based on the common effect configuration file. We assume that you have learned about OpenGL ES and shaders, so this topic does not describe them any more.

# Common effect configuration file

The effect resource package folder contains a common effect configuration file named `config.json` and some images. To use an effect, call the corresponding operation provided by the short video SDK and specify the directory of the effect resource package in the operation.

The common effect configuration file is a JSON file that contains the code of the complete effect rendering process.
The structure of this configuration file contains two hierarchies. The first hierarchy contains parameters for setting the basic information about an effect. The second hierarchy is the nodeTree parameter that specifies how to render the effect.

## 1. Basic information about an effect

The following table describes the basic information about an effect.

| Parameter | Description |
| -------- | -------- |
| name | The name of the effect. |
| module | The identifier of the module. Set the value to `ALIVC_GECF.` |
| version | The version number. The current version number is 1. |
| type | The type of the effect. Valid values: 1 and 2. The value 1 indicates a filter effect. The value 2 indicates a transition effect. |
| nodeTree | The node tree for specifying the effect rendering process. |

### Sample code 1

The structure of a common effect configuration file is as follows:

**Intense filter**

```JSON
{
    "name": "Intense",
    "module": "ALIVC_GECF",
    "version": 1,
    "type": 1,
    "nodeTree": [
        {
            "nodeId": 0,
            "name": "Intense",
            "fragment": "/** ...... */",
            "textures": [
                {
                    "name": "inputImageTexture",
                    "srcType": "INPUT_NODE",
                    "nodeId": 0
                },
                {
                    "name": "inputImageTexture2",
                    "srcType": "IMAGE",
                    "path": "color.png"
                }
            ]
        }
    ]
}
```

## 2. nodeTree

The `nodeTree` parameter specifies the effect rendering process. A node tree can have one or more nodes. The following figure shows the effect rendering process by using the short video SDK.
![1579421044372-c4926531-f4d4-4b8a-8a4c-c0df48080100.png](https://intranetproxy.alipay.com/skylark/lark/0/2020/png/37085/1581408810463-f2653f96-dccc-42f2-adb9-ebc974738b4b.png)

In the preceding figure, the rendering process is represented as a node tree.
The input node is used to import video sources. During recording, the node imports image streams collected by the camera. During editing, the node imports the current video stream. You can use multiple input nodes to render multiple videos. For example, you can render effects for two video clips by using two input nodes, namely, INPUT_NODE0 and INPUT_NODE1, to create a transition.
Rendered video streams are generated by the output node. In the preview mode, the output node sends the rendered video stream to the screen. In the production mode, the output node sends the rendered video stream to the encoder.
The node tree in the middle of the preceding figure represents the `nodeTree` parameter in the common effect configuration file. A node tree can have one or more rendering nodes. The key part of the common effect configuration file is the configuration of related nodes.

### Node

The common effect configuration file provides node parameters for rendering effects, including the shader code and related parameters required for customizing effects.
The following table describes the node parameters.

| Parameter | Description |
| -------- | -------- |
| nodeId | The ID of the node, used to identify the current node. |
| name | The name of the node. |
| vertex | The vertex shader code. |
| vertexPath | The path to the file of the vertex shader code. |
| attributes | The attributes of the vertex shader. |
| fragment | The fragment shader code. |
| fragmentPath | The path to the file of the fragment shader code. |
| textures | The textures of the fragment shader. |
| params | The attributes of the uniform variable of the fragment shader. |

#### Sample code 2

**Sample code of transition rendering**

```JSON
{
    "name": "Translate",
    "module": "ALIVC_GECF",
    "version": 1,
    "type": 2,
    "nodeTree": [
        {
            "nodeId": 0,
            "name": "Translate",
            "vertex": "/** ...... */",
            "attributes": [
                {
                    "name": "a_position",
                    "type": "POSITION"
                },
                {
                    "name": "a_texcoord",
                    "type": "TEXTURECOORD"
                }
            ],
            "fragment": "/** ...... */",
            "textures": [
                {
                    "name": "RACE_Tex0",
                    "srcType": "INPUT_NODE",
                    "nodeId": 0
                },
                {
                    "name": "RACE_Tex1",
                    "srcType": "INPUT_NODE",
                    "nodeId": 1
                }
            ],
            "params": [
                {
                    "name": "direction",
                    "type": "INT",
                    "value": [
                        0
                    ],
                    "maxValue": [
                        3
                    ],
                    "minValue": [
                        0
                    ]
                }
            ]
        }
    ]
}
```

The following describes some of the node tree parameters.

#### vertex

This parameter specifies the vertex shader code. This parameter plays the same role as the vertexPath parameter. Set either one of the two parameters.

- This parameter is not required. If you do not set this parameter, the short video SDK provides the following default code:

```JSON
attribute vec4 position;
attribute vec4 inputTextureCoordinate;
varying vec2 textureCoordinate;
void main()
{
    gl_Position = position;
    textureCoordinate = inputTextureCoordinate.xy;
}
```

#### vertexPath

This parameter specifies the path to the file of the vertex shader code. This parameter plays the same role as the vertex parameter. Set either one of the two parameters.

#### attributes

This parameter specifies the attributes of the vertex shader, including the `name` and `type` attributes.
The following table describes available vertex shader types.``
| Type | Description |
| -------- | --------|
| POSITION | The vertex coordinate. |
| TEXTURECOORD | The texture coordinate. |

- This parameter is not required. If you do not set this parameter, the short video SDK provides the following default code:

```JSON
[
    {
        "name": "position",
        "type": "POSITION"
    },
    {
        "name": "inputTextureCoordinate",
        "type": "TEXTURECOORD"
    }
]
```

#### fragment

This parameter specifies the fragment shader code. It plays the same role as the fragmentPath parameter. Set either one of the two parameters.

#### fragmentPath

This parameter specifies the path to the file of the fragment shader code. It plays the same role as the fragment parameter. Set either one of the two parameters.

#### textures

This parameter specifies the attributes of the `sampler2D` texture, as described in the following table.
| Attribute | Description |
| -------- | --------|
| name | The name of the texture. |
| srcType | The data source type of the texture. |
| nodeId | The ID of the node. You must set this parameter when the srcType parameter is set to CUSTOM_NODE or INPUT_NODE. |
| path | The relative path of the texture image. You must set this parameter when the srcType parameter is set to IMAGE. |

srcType
`This parameter specifies the data source of the texture to be used. The following table describes the valid values of this parameter.`

| Value | Description |
| -------- | -------- |
| IMAGE | Indicates that the texture image is from an effect resource folder. If you use this value, you must specify the relative path of the texture image in the `path` parameter and place the image in the resource package. |
| INPUT_NODE | Indicates that the texture image is from an input node. If you use this value, you must specify the node ID in the `nodeId` parameter. |
| CUSTOM_NODE | Indicates that the texture image is from an output node. If you use this value, you must specify the node ID in the `nodeId` parameter. |

- If the `srcType` parameter is set to `INPUT_NODE`, following these rules when you set the `nodeId` parameter: For filter effects, set the nodeId parameter to `0`. For transition effects, set the `nodeId` parameter to `0` if the texture image is from the prior video stream, and to `1` if the texture image is from the next video stream.``

Sample code 1 for an intense filter specifies two textures, namely, `inputImageTexture` and `inputImageTexture2`. The source of inputImageTexture is the input node `INPUT_NODE0`. The source of inputImageTexture2 is the image file `color.png`. Therefore, the textures parameter for the intense filter is set as follows:

```JSON
"textures": [
    {
        "name": "inputImageTexture",
        "srcType": "INPUT_NODE",
        "nodeId": 0
    },
    {
        "name": "inputImageTexture2",
        "srcType": "IMAGE",
        "path": "color.png"
    }
]
```

Sample code 2 for a transition effect specifies two textures, namely, `RACE_Tex0` and `RACE_Tex1`. The source of RACE_Tex0 is the input node `INPUT_NODE0`. The source of RACE_Tex1 is the input node `INPUT_NODE1`. Therefore, the textures parameter for the transition effect is set as follows:

```JSON
"textures": [
    {
        "name": "RACE_Tex0",
        "srcType": "INPUT_NODE",
        "nodeId": 0
    },
    {
        "name": "RACE_Tex1",
        "srcType": "INPUT_NODE",
        "nodeId": 1
    }
]
```

#### params

This parameter specifies the attributes of the `uniform` variable of the fragment shader.

| Attribute | Description |
| -------- | -------- |
| name | The name of the uniform variable. |
| type | The type of the uniform variable. |
| value | The value of the uniform variable. |
| maxValue | The maximum value of the uniform variable. |
| minValue | The minimum value of the uniform variable. |

type
The uniform variable supports the types in the following table.

| Type | Example |
| -------- | -------- |
| INT | [1] |
| FLOAT | [-2.0] |
| VEC2I | [3, 2] |
| VEC3I | [1, 2, 3] |
| VEC4I | [4, 3, 2, 1] |
| VEC2F | [-1.0, 1.0] |
| VEC3F | [0.5, 0.5, 0.5] |
| VEC4F | [1.0, -1.0, 1.0, -1.0] |
| MAT3F | [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0] |
| MAT4F | [1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0] |

Sample code 2 for a transition effect specifies a `uniform` variable named `direction`. The type is `INT`, the maximum value is 3, and the minimum value is 0. Therefore, the params parameter for the transition effect is set as follows:

```JSON
"params": [
    {
        "name": "direction",
        "type": "INT",
        "value": [
            0
        ],
        "maxValue": [
            3
        ],
        "minValue": [
            0
        ]
    }
]
```

## Built-in variables

The short video SDK provides the following built-in variables for configuring shaders:

```
uniform highp float BUILTIN_PROGRESS;	// The time progress. Valid values: 0 to 1.
uniform highp float BUILTIN_WIDTH;			// The image width.
uniform highp float BUILTIN_HEIGHT;			// The image height.
```

# Configure effects by using the short video SDK

## 1. Create effects

### Filter

**iOS**
You can call the `- (instancetype)initWithFile:(NSString *)path;` method of the `AliyunEffectFilter` class to create filter effects. The value of the `path` parameter is the path to the folder where effect resources are stored.
**Android**
You can call the `EffectFilter(String path)` method to create filter effects. The value of the `path` parameter is the path to the folder where effect resources are stored.

### Transition

**iOS**
You can call the `(instancetype)initWithPath:(NSString *)path` method of the `AliyunTransitionEffect` class to create transition effects. The value of the `path` parameter is the path to the folder where effect resources are stored.
**Android**
You can call the `TransitionBase(String path)` method to create transition effects. The value of the `path` parameter is the path to the folder where effect resources are stored.

## 2. Apply effects

### Filter

**iOS**
You can call the `- (int)applyAnimationFilter:(AliyunEffectFilter *)filter;` method to apply filter effects.

**Android**
You can call the `int addAnimationFilter(EffectFilter filter);` method to apply filter effects.

### Transition

**iOS**
You can call the `- (int)applyTransition:(AliyunTransitionEffect *)transition atIndex:(int)clipIdx;` method to apply transition effects.

**Android**
You can call the `int setTransition(int index, TransitionBase transition);` method to apply transition effects.

## 3. Modify effect parameters

After an effect object is initiated, an `AliyunEffectConfig` object is generated. The AliyunEffectConfig object describes the effect configuration with the same structure as that of the effect configuration file.
If a custom effect configuration file contains the `params` parameter, the `AliyunEffectConfig` object also contains an `AliyunParam` object in the `params` parameter of `nodeTree`. The `value` parameter of the `AliyunParam` object indicates the value of the specified uniform variable.

To modify an effect parameter, follow these steps:

1. Call the update method of the `AliyunValue` class to modify the value of the effect parameter.
2. Call one of the following methods as required to validate the modified effect parameter:

### Filter

**iOS**
Call the `- (int)updateAnimationFilter:(AliyunEffectFilter *)filter;` method to update a filter effect.

**Android**
Call the `int updateAnimationFilter(EffectFilter filter);` method to update a filter effect.

### Transition

**iOS**
Call the `-(int)updateTransition:(AliyunTransitionEffect *)transition atIndex:(int)clipIdx;` method to update a transition effect.

**Android**
Call the `int updateTransition(int clipIndex, TransitionBase transitionBase);` method to update a transition effect.
