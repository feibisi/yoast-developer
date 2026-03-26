---
title: "Recipe"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Pieces
    - Schema
    - Features
---


import YoastSchemaExample from '../../../../src/components/YoastSchemaExample';

描述一个 `Recipe`（菜谱），它包含一系列步骤说明、食材和可选字段。

## 触发条件
当页面内容描述一个菜谱时，应将其作为图谱中的顶级节点添加。

## 必需属性
一个有效的 `Recipe` 必须包含以下属性。

* `@type`: `Recipe`。
* `@id`: 网站的首页 URL 后追加 `#/schema/Recipe/{{ID}}`，其中 `{{ID}}` 是一个唯一标识符。
* `name`: 描述食谱的字符串。
* `image`: 代表成品食谱的图像，通过 ID 引用。
* `recipeIngredient`: 一个字符串数组，代表每种成分及其数量（例如，"3 apples"）。
* `recipeInstructions`: 一个 `HowToStep` 对象数组，包含以下属性：
  * `@type`: `HowToStep`。
  * `@id`: 页面未修改的 *规范 URL*，后追加 `#/schema/HowToStep/{{ID}}`，其中 `{{ID}}` 是一个唯一标识符。
  * `url`: 指向单个步骤的片段标识符（'ID 锚点'）的链接（例如，`https://www.example.com/example-page/#recipe-step-5`）。
  * `text`: 说明字符串（例如，"Bake at 200*C for 40 minutes, or until golden-brown, stirring periodically throughout"）。
  * `name`（可选）：步骤的简短摘要（例如，"Bake"）。
  * `image`（可选）：代表步骤的图像，通过 ID 引用。

## 故障场景
如果 `Recipe` 的任何必填字段缺失或无效，则不应输出该节点。

如果某个节点未被输出，则任何原本会声明与其存在关系的实体都应移除这些引用。

## 可选属性
以下属性在可用且有效时应添加：

* `description`：描述食谱的字符串。
* `cookTime`：烹饪时间，采用 ISO 8601 格式。
* `prepTime`：准备食谱所需的时间。
* `nutrition`：一个 `NutritionInformation` 节点，包含一个 `calories` 属性，该属性将卡路里计数定义为字符串（例如，"270 calories"）。
* `recipeYield`：食谱制作的份数（_不是_单个物品的数量，如果两者不同），作为字符串（例如，`"6"`，而不是 `6`）。
* `tools`：表示食谱所需工具的字符串数组。
* `keywords`：描述食谱的关键词数组。
* `recipeCuisine`：描述菜系类型的字符串（例如，"American" 或 "Spanish"）。
* `recipeCategory`：食谱的类别，从以下选项中选择：'开胃菜'、'早餐'、'早午餐'、'甜点'、'晚餐'、'饮品'、'午餐'、'主菜'、'酱汁'、'配菜'、'小吃'、'前菜'。
* `suitableForDiet`：一个 `RestrictedDiet` 节点，其值（或值数组）从以下选项中选择：'糖尿病饮食'、'无麸质饮食'、'清真饮食'、'印度教饮食'、'犹太洁食饮食'、'低卡路里饮食'、'低脂饮食'、'低乳糖饮食'、'低盐饮食'、'纯素饮食'、'素食饮食'。
* `video`：通过 ID 引用代表食谱说明的视频。
* `inLanguage`：指南的语言代码；例如，`en-GB`。
* `datePublished`：食谱添加的日期，采用 ISO 8601 格式。

## 条件属性

### 当菜谱是页面的“主要实体”时
* `mainEntityOfPage`: 通过 ID 引用 `WebPage`。

### 当仅指定 `cookTime` 或 `prepTime` 之一时
* 将相关属性更改为 `totalTime`。

### 当同时指定 `cookTime` 和 `prepTime` 时
* `totalTime`：`cookTime` 与 `prepTime` 之和，采用 ISO 8601 持续时间格式。

### 当页面包含有效的 `Article` 时
* 将 `mainEntityOfPage` 属性更改为通过 ID 引用 `Article`（而非 `WebPage`）。

#### 当 `Article` 有 `Author` 时
* `author`：通过 ID 引用 `Article` 的作者。

## Examples

### Minimum criteria

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Recipe",
            "mainEntityOfPage": {
                "@id": "https://www.example.com/#/schema/Article/abc123"
            },
            "name": "Party Coffee Cake",
            "image": {
                "@id": "https://www.example.com/uploads/example-image.jpg"
            },
            "recipeIngredient": [
                "2 cups of flour",
                "3/4 cup white sugar",
                "2 teaspoons baking powder",
                "1/2 teaspoon salt",
                "1/2 cup butter",
                "2 eggs",
                "3/4 cup milk"
            ],
            "recipeInstructions": [
                {
                    "@type": "HowToStep",
                    "text": "Preheat the oven to 350 degrees F. Grease and flour a 9x9 inch pan.",
                    "url": "https://example.com/example-page/#recipe-step-1"
                },
                {
                    "@type": "HowToStep",
                    "text": "In a large bowl, combine flour, sugar, baking powder, and salt.",
                    "url": "https://example.com/example-page/#recipe-step-2"
                },
                {
                    "@type": "HowToStep",
                    "text": "Mix in the butter, eggs, and milk.",
                    "url": "https://example.com/example-page/#recipe-step-3"
                },
                {
                    "@type": "HowToStep",
                    "text": "Spread into the prepared pan.",
                    "url": "https://example.com/example-page/#recipe-step-4"
                },
                {
                    "@type": "HowToStep",
                    "name": "Bake",
                    "url": "https://example.com/example-page/#recipe-step-5"
                },
                {
                    "@type": "HowToStep",
                    "text": "Allow to cool and enjoy.",
                    "url": "https://example.com/example-page/#recipe-step-6"
                }
            ]
        }
    ]
}`}
</YoastSchemaExample>

### 扩展标准

<YoastSchemaExample>
{`{
    "@context": "https://schema.org",
    "@graph": [
        {
            "@type": "Recipe",
            "mainEntityOfPage": {
                "@id": "https://www.example.com/#/schema/Article/abc123"
            },
            "name": "Party Coffee Cake",
            "image": {
                "@id": "https://www.example.com/uploads/example-image.jpg"
            },
            "video": {
                "@id": "https://www.example.com/#/schema/VideoObject/abc123"
            },
            "author": {
                "@id": "https://www.example.com/#/schema/Person/abc123"
            },
            "inLanguage": "en-GB",
            "datePublished": "2018-03-10",
            "description": "This coffee cake is awesome and perfect for parties.",
            "prepTime": "PT20M",
            "cookTime": "PT30M",
            "totalTime": "PT50M",
            "keywords": "cake for a party, coffee",
            "recipeYield": "10",
            "recipeCategory": "Dessert",
            "recipeCuisine": "American",
            "suitableForDiet": [
                "VegetarianDiet",
                "LowSaltDiet"
            ],
            "tools": [
                "A 9x9 inch pan",
                "A large mixing bowl"
            ],
            "nutrition": {
                "@type": "NutritionInformation",
                "calories": "270 calories"
            },
            "recipeIngredient": [
                "2 cups of flour",
                "3/4 cup white sugar",
                "2 teaspoons baking powder",
                "1/2 teaspoon salt",
                "1/2 cup butter",
                "2 eggs",
                "3/4 cup milk"
            ],
            "recipeInstructions": [
                {
                    "@type": "HowToStep",
                    "name": "Preheat",
                    "text": "Preheat the oven to 350 degrees F. Grease and flour a 9x9 inch pan.",
                    "url": "https://example.com/example-page/test/#recipe-step-1",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-2.jpg"
                    }
                },
                {
                    "@type": "HowToStep",
                    "name": "Mix dry ingredients",
                    "text": "In a large bowl, combine flour, sugar, baking powder, and salt.",
                    "url": "https://example.com/example-page/test/#recipe-step-2",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-3.jpg"
                    }
                },
                {
                    "@type": "HowToStep",
                    "name": "Add wet ingredients",
                    "text": "Mix in the butter, eggs, and milk.",
                    "url": "https://example.com/example-page/test/#recipe-step-3",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-4.jpg"
                    }
                },
                {
                    "@type": "HowToStep",
                    "name": "Spread into pan",
                    "text": "Spread into the prepared pan.",
                    "url": "https://example.com/example-page/test/#recipe-step-4",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-5.jpg"
                    }
                },
                {
                    "@type": "HowToStep",
                    "name": "Bake",
                    "text": "Bake for 30 to 35 minutes, or until firm.",
                    "url": "https://example.com/example-page/test/#recipe-step-5",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-6.jpg"
                    }
                },
                {
                    "@type": "HowToStep",
                    "name": "Enjoy",
                    "text": "Allow to cool and enjoy.",
                    "url": "https://example.com/example-page/test/#recipe-step-6",
                    "image": {
                        "@id": "https://www.example.com/uploads/example-image-7.jpg"
                    }
                }
            ]
        }
    ]
}`}
</YoastSchemaExample>