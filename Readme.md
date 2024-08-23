
# StyledLines
GPT2-Based Text Stylization Model with Llama.cpp Wrapper in form of a Unity3d Asset Store package avaliable [here: https://assetstore.unity.com/packages/slug/292902](https://u3d.as/3mfu)

This repository provides a GPT2-based text stylization LLM model, wrapped with Llama.cpp to ensure compatibility across various platforms, including iOS and WebGL. The model is designed to transform generic texts into stylized, game or user-tailored dialogue.

## purpose of this repo
Please do post issues you find in our docs/code/other stuff here.

## Features

- **Text Rewriting**: A GPT2-based line rewriter for stylizing texts.
- **Cross-Platform**: Compatible with WebGL, iOS, Android, Windows x64, and MacOS (ARM).
- **Async and Sync Execution**: Supports asynchronous execution using C++20 threads and synchronous execution as well.
- **Customizable**: Allows configuration of Llama.cpp parameters directly within the UnityEditor UI, with setup and execution callbacks.
- **Quantised models on-board**: We this package with three model quantised resolutions
   - **_Q4_K_M_** most heavily quantised (simplified) one, 110mb
   - **_Q8_0_** 175mb
   - **_bf16_** original resolution model, 321mb

## Try It Online

Experience the model directly in your browser (with the **_Q4_K_M_** model):

- [Automated NPC Dialogue Stylization Demo](https://d23myu0xfn2ttc.cloudfront.net/rich/index.html)
- [Single Prompt Debug Demo](https://d23myu0xfn2ttc.cloudfront.net/)

## Model Usage Example

Provide the following input:
```plaintext
<input> How are you today? <inputEnds>
<style> Pirate's Orwellian and Poetic Question <styleEnds>
<output>
```
And it will generate:
```plaintext
Hey there, how fares ye today?
```

## Model Information

- **Training Data**: Finetuned on a corpus of over 0.5 million dialogue lines (64,343,812 tokens) synthetically generated using GPT-4.
- **Tools**: Output filtering tools provided in C# for easy editing.
- **Wrapper**: Built on Llama.cpp release tag b3490, supporting the latest Llama 3.1 405b model.
- **Model Formats**: Weights are available in bf16, int8, q4_k_m resolutions in .gguf format.

## Recommendations

To get the best results, consider using the following styles:
- **Mood Styles**: Inquisitive, Emotional, Intellectual, Dynamic, Noble, Light, Foreboding.
- **Writing Styles**: Historical, Modern and Contemporary, Genre-Specific, Expressive and Creative.

**Tip**: Keep your style descriptions short and concise. Combining mood and writing styles often yields the best results.

## Integration Notes

- **WebGL**: Limited to 4GB of RAM (x86), and published pages may not work on mobile devices with less RAM (e.g., iOS 17).
- **MacOS/iOS Builds**: Requires Mac OS-based computers with CMake and Xcode installed.
- **AI Output**: The LLM model may generate tokens that are not parsable by certain fonts.
- **Model Performance**: Optimized for one-liners and single sentences.
- **CPP-docs**: [avaliable here](https://testedlines.github.io/StyledLines/cpp-docs/index.html), helpfull for rebuilding wrappers
- **CSharp (this docs)**: [avaliable here](https://testedlines.github.io/StyledLines/csharp-docs/index.html)
## License

The model and code are released under Unity3D Asset Store’s standard End User License Agreement. See the full EULA [here](https://assetstore.unity.com/browse/eula-faq).

## Coding

### Motivating usage example:

```Csharp
using UnityEngine;
using System.Collections;

public class MinimalExample : MonoBehaviour
{
    public BinaryAsset binaryFileAsset;

    private ModelController _llmController = null;
    private void Start()
    {
        _llmController = new ModelController(binaryFileAsset);
        StartCoroutine(GenerateLine());
    }

    private IEnumerator GenerateLine()
    {
        string line = "I love you!";
        string style = "Simple, Confident, Shakespearean";

        var prompt = $"<input> {line} <inputEnds>\\n<style> {style} <styleEnds>\\n<output>";
        var id = _llmController.model.GenerateAsync(prompt);

        while (!_llmController.model.IsGenerationReady(id))
        {
            yield return new WaitForSeconds(0.3f);
        }

        string result = _llmController.model.GetGenerationResults(id);
        result = ModelController.ExtractFirstPart(result, "<out");
        Debug.Log(result); // prints out something like "My heart is boundless with affection!"
    }
}
```


