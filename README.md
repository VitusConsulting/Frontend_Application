# Frontend_Application


pip install "reactpy[starlette]"

# component for creating function that returns HTML-like React components, 
# html for website elements, 
# run for launching your app.
from reactpy import component, html, run 

# Define your component
@component
def HelloWorld():
    return html.h1("Hello, World!")

# Run it with a development server. For testing purposes only.
run(HelloWorld)

from reactpy import component, html, run

@component
def Title(title):
    return html.h1(title)

# Load an image from https://picsum.photos/id/152/500/300,
# specify style of the image using CSS: width: 30%
@component
def Photo():
    return html.img(
        {
            "src": "https://picsum.photos/id/152/500/300",
            "style": {"width": "30%"},
        }
    )

@component
def PhotographerName(caption):
    return html.h4(caption)

# Combine all previous components in one function
@component
def PhotoViewer():
    return html.section(
        Title("Photo of the day"),
        Photo(),
        PhotographerName("Steven Spassov")
    )

from reactpy import component, html, run

# Define a function that return a button, which on clicking executes
# Python function "handle_event" (prints some text)
@component
def PrintButton(display_text, message_text):
    def handle_event(event):
        print(message_text)

    return html.button({"on_click": handle_event}, display_text)

@component
def App():
    return html.div(
        PrintButton("Play", "Playing"),
        PrintButton("Pause", "Paused"),
    )

run(App)
run(PhotoViewer)


import json
from pathlib import Path

from reactpy import component, hooks, html, run

# Define current script directory, save path to the data.json file in DATA_PATH,
# load this json file in sculpture_data (a dictionary)
HERE = Path(__file__)
DATA_PATH = HERE.parent / "data.json"
sculpture_data = json.loads(DATA_PATH.read_text())


@component
def Gallery():
    # Set React states
    index, set_index = hooks.use_state(0)
    
    # Function that will be ran after pressing the button.
    # It changes the index, so we can keep track of current state.
    def handle_click(event):
        set_index(index + 1)
    
    # Index shouldn't be bigger than amount of elements in data
    bounded_index = index % len(sculpture_data)
    # Take the data from dict
    sculpture = sculpture_data[bounded_index]
    alt = sculpture["alt"]
    artist = sculpture["artist"]
    description = sculpture["description"]
    name = sculpture["name"]
    url = sculpture["url"]

    return html.div(
        html.button({"on_click": handle_click}, "Next"),
        html.h2(name, " by ", artist),
        html.p(f"({bounded_index + 1} of {len(sculpture_data)})"),
        html.img({"src": url, "alt": alt, "style": {"height": "200px"}}),
        html.p(description),
    )


run(Gallery)
