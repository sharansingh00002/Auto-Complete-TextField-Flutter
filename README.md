# auto_complete_text_view

A simple yet customizable autoCompleteTextField for flutter

## Example Code
```
getSuggestionsListFunction : 

  Future<List<String>> getLocationSuggestionsList(String locationText) async {
    final bloc = BlocProvider.of<EditProfileBloc>(context);
    List<String> suggestionList = List();
    LocationModel data = await bloc.fetchLocationSuggestions(locationText);
    if (data != null) {
      for (Predictions predictions in data.predictions) {
        suggestionsKeyValuePairs[predictions.description] = predictions.placeId;
        if (!suggestionList.contains(predictions.description))
          suggestionList.add(predictions.description);
      }
      return suggestionList;
    } else {
      return [''];
    }
  }



function declaration : 
                     AutoCompleteTextView(
                        suggestionsApiFetchDelay: 300,
                        focusGained: () {},
                        onTapCallback: (_) async {
                          locationSavedStatus = LocationSaveStatus.inProgress;
                          saveLocationValuesFromGeoCoding(bloc).then(
                            (_) =>
                                locationSavedStatus = LocationSaveStatus.saved,
                          );
                        },
                        focusLost: () {
                          locationSavedStatus = LocationSaveStatus.saved;
                          if (locationTextController.text.isEmpty) {
                            city = '';
                            state = '';
                            country = '';
                            locationTextController.text = '';
                          } else {
                            locationTextController.text = getLocationString(
                                country: country, state: state, city: city);
                          }
                        },
                        onValueChanged: (String text) {
                          locationSavedStatus = text.isNotEmpty
                              ? (getLocationString(
                                              city: '', state: '', country: '')
                                          .trim() ==
                                      text.trim())
                                  ? LocationSaveStatus.saved
                                  : LocationSaveStatus.notSaved
                              : LocationSaveStatus.saved;
                        },
                        controller: locationTextController,
                        suggestionStyle: Theme.of(context).textTheme.body1,
                        getSuggestionsMethod: getLocationSuggestionsList,
                        tfTextAlign: TextAlign.left,
                        tfStyle: TextStyle(
                          fontSize: 16,
                          color: Theme.of(context).textTheme.body1.color,
                        ),
                        tfTextDecoration: InputDecoration(
                          border: InputBorder.none,
                          hintText: msgLocationHint1,
                        ),
                      ),
```
