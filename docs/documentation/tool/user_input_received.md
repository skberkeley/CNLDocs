# hilt.Tool.user_input_received

`hilt.Tool.user_input_received()`

Returns whether user input has been received for the particular `Stage` being run. Intended to be used inside SDFs, to separate code to be run after user input has been received, such as to process it.

### Returns

`True` if user input has been received when this function was called, otherwise `False`.