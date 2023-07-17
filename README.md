# COATL-LIGM
## Conception d'un Outil d'Alignement de Textes Littéraires - Laboratoire d'Informatique Gaspard-Monge
> Coatl \[ˈkoː.aːt͡ɬ] (Classical Nahuatl) : a snake, a serpent

### About
COATL is a text alignment tool. It displays the detailed difference between two given texts.
The default setting is to compare the texts word-to-word, a character-to-character setting is planned for the very near future.
These differences can be of many natures, and the user gets to decide what types of differences the tool should ignore.
Most of the algorithm's functionalities are entirely written in Javascript within the html page, meaning it stays usable when offline if downloaded.
The tool was made with french litterary texts in mind (17th to 21st centuries), but can be used with any unicode texts.

### Steps of the algorithm
The baseline is Levenshtein distance, which uses a matrix of scores and pathfinding to find the shortest sequence of modifications between two texts (or any other type of sequences of values).

#### Text division and regular expressions
Before the alignment algorithm is applied, the text needs to be divided into words.
Regular expressions are used to separate "letter" unicode characters from the other ones.
The words are then put into an array, with the punctuation sequences too if chosen by the user.

#### The multi-scale algorithm
To avoid full quadratic time complexity, we split the matrix into a grid of smaller matrixes, and apply the same algorithm on a bigger scale to filter out the cells of the grid we don't need to process.
Instead of the [0 / 1] Levenshtein score, we use Jaccard distance, in the form of its simplified algorithm : minHash.

#### Levenshtein distance
For every cell of the full matrix, we compare the two corresponding words and give them a score between 0 and 1.
We then propagate these costs in the matrix using the algorithm for Levenshtein distance.
Our way of doing it is different as we multiply the "local" score by two to avoid useless substitutions.

#### Word comparing and databases
To calculate the score between 0 and 1, we apply multiple layers of comparisons.
For example, if the words aren't the same, but only because of a type case difference, they get a score of 0.1 instead of 1.
The user can influence these comparisons through options on the text entry display.