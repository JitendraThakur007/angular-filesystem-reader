#Overview
The script takes input in the order of: input file, output file, address field id, longitude id, lattitude id, limit. It will abort if input file is not found, or if output file already exists. 

#Prerequisites
You need to have node and npm installed. A suitable installer can be found on the [official node website](https://nodejs.org/en/download/).

#How to setup dependencies.
In the command line, move to the directory that contains the index.js file in this submission and run:
    npm install

#How to setup configuration
Configuration can is in the file called config.js. The input file delimiter, the output file delimiter, and the logs folder can be configured.

#Sample usage
Place the input.csv file in the root directory, and run:
    node index.js "input.csv" "output.csv" 0 1 2 1

This basically means, read input.csv where the 0th index is the address, the 1st index of each row is the longitude, the 2nd is latitude and the limit for API calls is 1. The input file has coordinates missing for 2 rows, but only one will be fixed because the limit is set to 1, if the limit is increased, the coordinates of both will be fixed. Note that the order of the rows might change, but the coordinates will remain consistent with the address.
