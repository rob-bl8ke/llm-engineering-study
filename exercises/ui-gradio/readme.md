
# [Gradio](https://www.gradio.app/)
- [Quickstart Guide](https://www.gradio.app/guides/quickstart)

To use Gradio, import it.
```python
import gradio as gr
```

The simplest way to use it to pass in a callback function which accepts input via a textbox and will output whatever the callback function returns to an output textbox.
```python
gr.Interface(fn=shout, inputs="textbox", outputs="textbox", flagging_mode="never").launch()
```

### Public sharing
- Adding `share=True` means that it can be accessed publically
- A more permanent hosting is available using a platform called [Spaces](https://huggingface.co/spaces) from HuggingFace
> NOTE: Some Anti-virus software and Corporate Firewalls might not like you using `share=True`. 
- ⚠️ Watch out if you're at work on on a work network.

```python
gr.Interface(fn=shout, inputs="textbox", outputs="textbox", flagging_mode="never").launch(share=True)
```

#### Authentication
If you're sharing publically, you may wish to add a username and password. You'd need to use a secure location to store passwords. At a minimum, use your `.env` file.

### Launch in browser
If you're running this code in a python file and not in jupyter notebooks (Anaconda3) or using uv in VS Code for instance, you may wish to automatically launch your Gradio interface in the browser.
```python
# Adding inbrowser=True opens up a new browser window automatically
gr.Interface(fn=shout, inputs="textbox", outputs="textbox", flagging_mode="never").launch(inbrowser=True)
```
