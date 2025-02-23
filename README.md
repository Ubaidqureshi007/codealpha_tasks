# Import required libraries for the translation interface
# ipywidgets for creating interactive widgets in Jupyter notebooks
# googletrans for translating text, using version 4.0.0-rc1 for stability
import ipywidgets as widgets
from IPython.display import display
from googletrans import Translator, LANGUAGES

# Install the googletrans library if not already installed
# This command is typically run in a Jupyter cell with !pip
# !pip install googletrans==4.0.0-rc1

def translate_text(input_text, target_language_code):
    """
    Translate the given text into the specified target language.

    Args:
        input_text (str): The text to translate.
        target_language_code (str): The language code for the target language (e.g., 'ur' for Urdu).

    Returns:
        str: The translated text, or an error message if translation fails.
    """
    # Create a Translator instance
    translator = Translator()
    
    try:
        # Attempt to translate the text
        translation = translator.translate(input_text, dest=target_language_code)
        return translation.text
    except Exception as error:
        # Handle any errors (e.g., network issues, invalid language code)
        return f"Translation failed: {str(error)}"

def handle_translation(button):
    """
    Handle the translation button click event.
    Validates input, translates text, and updates the output label.

    Args:
        button: The button widget that triggered this callback.
    """
    # Get the user input and target language
    user_text = text_input_field.value.strip()
    selected_language = language_selector.value
    
    # Check if the user entered any text
    if not user_text:
        output_display.value = "Please enter some text to translate."
        return
    
    # Check if a target language was selected
    if not selected_language:
        output_display.value = "Please select a target language from the dropdown."
        return
    
    # Translate the text and update the output
    translated_result = translate_text(user_text, selected_language)
    output_display.value = f"Translated Text: {translated_result}"

# Create interactive widgets for the translation interface
text_input_field = widgets.Text(
    value='',  # Start with an empty field
    placeholder='Type the text you want to translate here...',  # Friendly placeholder
    description='Text to Translate:',  # Clear label
    disabled=False,  # Allow user input
    layout={'width': '500px'}  # Make the input field wider for better usability
)

language_selector = widgets.Dropdown(
    options=list(LANGUAGES.keys()),  # List of language codes from googletrans
    value='ur',  # Default to Urdu for the example
    description='Translate to:',  # Clear and concise label
    disabled=False,  # Allow selection
    layout={'width': '300px'}  # Adjust width for readability
)

translate_button = widgets.Button(
    description='Translate Now',  # More descriptive button text
    disabled=False,  # Enable the button
    button_style='success',  # Green button for positive action
    tooltip='Click to translate the text into the selected language'  # Helpful tooltip
)

output_display = widgets.Label(
    value="",  # Start with no output
    layout={'margin': '10px 0'}  # Add spacing for better visual separation
)

# Connect the button to the translation handler
translate_button.on_click(handle_translation)

# Display the widgets in a user-friendly layout
display(widgets.VBox([
    text_input_field,
    language_selector,
    translate_button,
    output_display
]))
