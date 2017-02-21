![alt tag](https://raw.githubusercontent.com/AshokHub/BLASTphp/misc/BLASTphp_Logo_500px.png)
    
# [About](https://github.com/AshokHub/BLASTphp/blob/master/README.md)
The [BLASTphp](https://github.com/AshokHub/BLASTphp) package is a PHP wrapper for the [NCBI BLAST Common URL API](https://ncbi.github.io/blast-cloud/dev/api.html). It allows you to run [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) RESTful web services remotely.

# [Usage](https://ncbi.github.io/blast-cloud/doc/running-web-blast.html)
Please refer to [NCBI BLAST Cloud Documentation](https://ncbi.github.io/blast-cloud/) for detailed information.

To use BLASTphp, first include the galaxy.inc file in your program.  For example:

    require_once('[BLASTphp installation dir]/galaxy.inc');

Where [BLASTphp installation dir] is where the BLASTphp package is installed. 

# [Example](https://github.com/AshokHub/BLASTphp#example)
## [Connecting to an existing Galaxy Server](https://github.com/AshokHub/BLASTphp#connecting-to-an-existing-galaxy-server)
The following is an example script that can be executed on the command-line. It is contained in the examples directory of this repository and named 'check_job_status.php'.  Typically PHP scripts are not used on the command-line but here we do so for the sake of demonstration.  The following script receives as its first argument the API key of a user on the public BLAST server at https://biogem.org.  It authenticates the user using the API key, queries the remote server to find the list of workflows that the user has created and prints the names of those workflows.

    <?php
    
    // Include the BLASTphp library.
    require_once('../blast.inc');
    
    // The domain name of the host to connect to.
    $hostname  = 'biogem.org';
    // Port 443 is the typical port for HTTPS.
    $port      = '443';
    }

# [Usage Guidelines](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=DeveloperInfo)
Do not overload the NCBI servers. If you are intending to perform more than 20 searches in a session you should comply with the following guidelines:
	
# [License](https://github.com/AshokHub/BLASTphp/blob/master/LICENSE)
BLASTphp is made available under version 3 of the GNU Lesser General Public License.
