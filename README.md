![BLASTphp](https://raw.githubusercontent.com/AshokHub/BLASTphp/misc/BLASTphp_Logo_500px.png)

# [About](../master/README.md)
The [BLASTphp](https://github.com/AshokHub/BLASTphp) library is a PHP wrapper for the [NCBI BLAST URL API](https://ncbi.github.io/blast-cloud/dev/api.html). It allows remote execution of the [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) through RESTful services. [BLASTphp](https://github.com/AshokHub/BLASTphp) requests to [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) through HTTP/HTTPS interface and elicits a response in HTML, Text, XML, XML2, JSON2, or Tabular (text) format. The default response format is HTML.

Since [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) is a shared resource, usage limitations apply. [BLASTphp](https://github.com/AshokHub/BLASTphp) is a lightweight program which consumes less bandwidth and resource. Projects that involve a large number of BLAST searches should use the RESTful interface at [Cloud BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=CloudBlast) or stand-alone BLAST. Currently [NCBI](https://www.ncbi.nlm.nih.gov/) provides a commercial BLAST server image hosted in [Amazon Web Services (AWS)](https://aws.amazon.com/marketplace/pp/B00N44P7L6), [Google Compute Engine (GCE)](https://googlegenomics.readthedocs.org/en/latest/use_cases/run_familiar_tools/ncbiblast.html), and [Microsoft Azure](https://azure.microsoft.com/en-us/marketplace/virtual-machines/all/?term=ncbi-blast) cloud servers. This allows users to run stand-alone searches with the BLAST+ applications, submit searches through a subset of the [NCBI BLAST URL API](https://ncbi.github.io/blast-cloud/dev/api.html), and perform searches with a simplified webpage. The server image includes a [FUSE client](https://ncbi.github.io/blast-cloud/doc/fuse.html) that will download BLAST databases during the first search. The server image runs on Ubuntu Linux.

# [Usage](https://ncbi.github.io/blast-cloud/doc/running-web-blast.html)
Please refer to [NCBI BLAST URL API Documentation](https://ncbi.github.io/blast-cloud/dev/api.html) for setting BLAST parameters. If a parameter is not required and not provided, then the default value will be used. That default value may depend upon the BLAST search you are running. [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) can be executed by simply passing `CMD`, `PROGRAM`, `DATABASE`, and `QUERY` parameters to the [NCBI BLAST URL API](https://ncbi.github.io/blast-cloud/dev/api.html).

To use [BLASTphp](https://github.com/AshokHub/BLASTphp) through a webserver, first set the maximum execution time of webserver to request time of execution (RTOE) value of BLAST, because the default maximum execution time (30 seconds) is not enough. For example,

```php
ini_set('max_execution_time', $RTOE);
```

In some cases `$RTOE` value will be not enough to execute the program. If so, the `$RTOE` value must be increased to `$RTOE+60` or higher. A `max_execution_time` value of `0` will make the max execution time to unlimited. However, it is not recommended that you do this. In rare cases in which a script has somehow gone into an infinite loop, or is in a deadlock because of file level locking, your server will get overloaded and your memory and CPU usage will go above recommended thresholds.

The query sequence must be encoded before passing through `QUERY` parameter. For example,

```php
$encoded_query = urlencode($sequence);
```

Where `urlencode()` is the built-in PHP function to encode the non-alphanumeric characters to equivalent URL codes.

# [Example](https://github.com/AshokHub/BLASTphp#example)
## [Connecting to NCBI BLAST Server](https://github.com/AshokHub/BLASTphp#connecting-to-ncbi-blast-server)
The following is an example script to build the requests to [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi).

```php
<?php
$data = array('CMD' => 'Put', 'PROGRAM' => 'blastp', 'DATABASE' => 'pdb', 'QUERY' => $encoded_query);
$options = array(
  'http' => array(
    'header'  => "Content-type: application/x-www-form-urlencoded\r\n",
    'method'  => 'POST',
    'content' => http_build_query($data)
  )
);
$context  = stream_context_create($options);
$result = file_get_contents("https://blast.ncbi.nlm.nih.gov/blast/Blast.cgi", false, $context);
?>
```
	
The response may consist of `RID = VALUE`, `RTOE = VALUE`, `Informational`, `QBlastInfoBegin`, `QBlastInfoEnd`, `Status=WAITING`, `Status=FAILED`, `Status=UNKNOWN`, and/or `Status=READY` commands, which are used to track the result.

## [Retrieving from NCBI BLAST Server](https://github.com/AshokHub/BLASTphp#retrieving-from-ncbi-blast-server)
The following is an example script to retrieve response from the [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi).

```php
<?php
$option = array(
  'http' => array(
  	'method' => 'GET'
  )
);
$content = stream_context_create($option);
$output = file_get_contents("https://blast.ncbi.nlm.nih.gov/blast/Blast.cgi?CMD=Get&RID=$rid", false, $content);
?>
```

After successful execution, [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) returns response in HTML (*default*) format. To get different types of response, you must specify the file type in the URL. For example, `FORMAT_TYPE=Text` for plain text file format.

The complete working PHP script '[blastphp.php](../master/blastphp.php)' is included in the main directory of this repository.

# [Usage Guidelines](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=DeveloperInfo)
Do not overload the NCBI servers. If you are intending to perform more than 20 searches in a session you should comply with the following guidelines:

1. Do not contact the server more often than once every three seconds.
2. Do not poll for any single RID more often than once a minute.
3. Use the URL parameter email, and tool, so that we can track your project and contact you if there is a problem.
4. Run scripts weekends or between 9 pm and 5 am Eastern Time weekday if more than 50 searches will be submitted.

[NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) often runs more efficiently if multiple queries are sent as one search than if each query is sent as an individual search. This is especially true for *blastn*, *megablast*, and *tblastn*. For short queries (less than a few hundred bases), it is suggested to merge them into one search of up to 10,000 bases.

The [NCBI](https://www.ncbi.nlm.nih.gov/) servers are a shared resource and not intended for projects that involve a large number of BLAST searches. Stand-alone BLAST and the RESTful API at a cloud provider are provided for such projects.

# [Error Handling](https://github.com/AshokHub/BLASTphp#error-handling)
If there is a problem with a request, [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) REST (REpresentational State Transfer) will usually return some sort of human-readable message indicating what went wrong – whether it’s an invalid input, or nothing was found for the given query, or the request was too broad and took too long to complete (more than 30 seconds, the [NCBI](https://www.ncbi.nlm.nih.gov/) standard time limit on web service requests), etc.

If the operation was successful, the HTTP status code will be 200 (OK). If the server encounters an error, it will return an HTTP status code that gives some indication of what went wrong; possibly along with, depending on the output format (such as in a <Fault> tag in XML), some additional more human-readable detail message(s). The codes in the 400-range are errors on the client side, and those in the 500 range indicate a problem on the server side; the codes currently in use are:

| HTTP Status | Error Code | General Error Category |
|    :---:    |    :---:   | ---------------------- |
| 200 | (none) | Success |
| 202 | (none) | Accepted (asynchronous operation pending) |
| 400 | Bad Request | Request is improperly formed (syntax error in the URL, POST body, etc.) |
| 404 | Not Found | The input record was not found (e.g. invalid RID) |
| 405 | Method Not Allowed | Request not allowed (such as invalid MIME type in the HTTP Accept header) |
| 504 | Timeout | The request timed out, from server overload or too broad a request |
| 501 | Unimplemented | The requested operation has not (yet) been implemented by the server |
| 500 | Server Error | Some problem on the server side (such as a database server down, etc.) |
| 500 | Unknown | An unknown error occurred |

# [Support](https://github.com/AshokHub/BLASTphp#support)
Please feel free to sent your queries, suggestions and/or comments related to [BLASTphp](https://github.com/AshokHub/BLASTphp) program to [ashok.bioinformatics@gmail.com](ashok.bioinformatics@gmail.com) or [ashok@biogem.org](ashok@biogem.org).


# [License](https://github.com/AshokHub/BLASTphp/blob/master/LICENSE)
[BLASTphp](https://github.com/AshokHub/BLASTphp) is made available under version 3 of the GNU Lesser General Public License.

# [Citation](https://github.com/AshokHub/BLASTphp#citation)
Ashok Kumar, T., and Rajagopal, B. (2017). BLASTphp: a PHP wrapper for NCBI BLAST API. [*International Journal for Computational Biology*](http://ijcb.in). 6(1): 31-33. [[Abstract]](http://ijcb.in/ijcb/v1/index.php/ijcb/article/view/75) [[PDF]](http://ijcb.in/ijcb/v1/index.php/ijcb/article/viewFile/75/82)
