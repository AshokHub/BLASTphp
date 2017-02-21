![alt tag](https://raw.githubusercontent.com/AshokHub/BLASTphp/misc/BLASTphp_Logo_500px.png)
    
# About
The [BLASTphp](https://github.com/AshokHub/BLASTphp) package is a PHP wrapper for the [NCBI BLAST Common URL API](https://ncbi.github.io/blast-cloud/dev/api.html). It allows you to run [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) RESTful web services remotely.

# Usage
Please see the [API documentation page](http://AshokHub.github.io/BLASTphp/docs-v0.1a/html/index.html) for full information.

To use BLASTphp, first include the galaxy.inc file in your program.  For example:

    require_once('[BLASTphp installation dir]/galaxy.inc');

Where [BLASTphp installation dir] is where the BLASTphp package is installed. 

# Example
## Connecting to an existing Galaxy Server
The following is an example script that can be executed on the command-line. It is contained in the examples directory of this repository and named 'check_job_status.php'.  Typically PHP scripts are not used on the command-line but here we do so for the sake of demonstration.  The following script receives as its first argument the API key of a user on the public Galaxy server at https://usegalaxy.org.  It authenticates the user using the API key, queries the remote server to find the list of workflows that the user has created and prints the names of those workflows.

    <?php
    
    // Include the BLASTphp library.
    require_once('../blast.inc');
    
    // The domain name of the host to connect to.
    $hostname  = 'biogem.org';
    // Port 443 is the typical port for HTTPS.
    $port      = '443';
    }

# License
BLASTphp is made available under version 3 of the GNU Lesser General Public License
