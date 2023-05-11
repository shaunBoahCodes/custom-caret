# custom-caret
A Vue 3 component made with Composition API. It is a contenteditable span that does a fair share of manipulation in javascript which allows it to have a custom shaped caret using pseudo-elements.

The custom caret comes with full functionality including:
1. Moving with left and right arrow keys --moveIt()
2. Focusing the caret on the character that was clicked --select()
3. Focusing the caret on the last character when the background div 'wrapper' is clicked  --reset()
4. Caret follows the amount of characters typed (when backspaced, caret will update accordingly)
5. Caret stays in the correct place when adding text after moving the caret using arrow keys
6. When a text is selected, caret focuses to the last character of the selection
7. When a selected text is deleted, caret focuses to the first character before the selected text --select()
