<html>
 <head>
  <meta charset="UTF-8">
  <title>Call Api PHP</title>




 </head>
 <body>
 <?php 
$mytoken = "";
/////////////////////////////////////
$data_array =  array(
      "User"        => "Mohammad",
      "Pass"         => "123"
);

$make_call = callAPINEW('POST', 'http://192.168.1.20:4000/api/Authentication', json_encode($data_array));


//echo "testttttt";
//var_dump($make_call['body']);
//echo "testttttt";


//var_dump(json_decode($make_call['body']));

$obj = json_decode($make_call['body']);


$mytoken = $obj->{'Token'};
print "mytoken";
print "~~~~";
print $mytoken;

//var_dump($make_call['cookies']);

//print $make_call['cookies'];


print "~~~~";


$objkoo = json_decode($make_call['cookies']);

//$mykookkkk = $objkoo ->{'ASP_NET_SessionId'};
//print $mykookkkk;


foreach ($objkoo as $key=>$value) {  
     //echo "<td><h3>".$key."</h3></td>";
$mycookiname = $key;
$mycookivalue = $value;
   }

//print $mycookiname;
//print "~~~~";
//print $mycookivalue;






//////////////////////////////////////////
$data_arrayfish =  array(
      "Token"        => $mytoken,
      "EmpNumber"     => "1400"
);

$make_callFish = callAPI('POST', 'http://192.168.1.20:4000/GetData', json_encode($data_arrayfish),$mycookiname,$mycookivalue );




//echo $make_callFish;

//var_dump(json_decode($make_callFish));
var_dump(json_decode($make_callFish,true));

$objFish = json_decode($make_callFish);

//print $objFish->{'Result'}; 
//////////////////////////////////////////

//$pdf_base64 = $objFish->{'Result'};

//$pdf_base64_handler = fopen($pdf_base64,'r');
//$pdf_content = fread ($pdf_base64_handler,filesize($pdf_base64));
//fclose ($pdf_base64_handler);
//Decode pdf content
$pdf_decoded = base64_decode ( $objFish->{'Result'});
//Write data back to pdf file
$pdf = fopen ('Fish.pdf','w');
fwrite ($pdf,$pdf_decoded);
//close output file
fclose ($pdf);
echo "<iframe src=\"Fish.pdf\" width=\"100%\" style=\"height:100%\"></iframe>";

////////////////////////////////







function callAPI($method, $url, $data,$Authcookiname,$Authcookivalue){

   $curl = curl_init();



   switch ($method){
      case "POST":
         curl_setopt($curl, CURLOPT_POST, 1);
         if ($data)
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
         break;
      case "PUT":
         curl_setopt($curl, CURLOPT_CUSTOMREQUEST, "PUT");
         if ($data)
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);                       
         break;
      default:
         if ($data)
            $url = sprintf("%s?%s", $url, http_build_query($data));
   }
//print " positionnnnnn ";
//print $Authcookiname;
$position = strpos($Authcookiname, '_');
//print $position;
$Authcookiname[$position] = '.';
print "Authcookiname";
print "~~~~";
print $Authcookiname;
print "~~~~";
print "Authcookivalue";
print "~~~~";
print $Authcookivalue;
print "~~~~";

   // OPTIONS:
curl_setopt($curl, CURLOPT_COOKIE, $Authcookiname. '=' .$Authcookivalue);
//curl_setopt($curl, CURLOPT_COOKIE, "ASP.NET_SessionId=".$Authcookivalue);



   curl_setopt($curl, CURLOPT_URL, $url);



curl_setopt($curl, CURLOPT_ENCODING, "deflate, gzip");

curl_setopt($curl, CURLOPT_HTTPHEADER, array(
      
      'Content-Type: application/json',
   ));



//curl_setopt($curl, CURLOPT_COOKIE, "$Authcookiname=$Authcookivalue");

   




//$headersss[] = 'Cookie: $Authcookiname=$Authcookivalue';

//curl_setopt($curl, CURLOPT_HTTPHEADER, $headersss);



curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
   curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);



      
//print "-------";
//print $Authcookiname;
//print "-------";
//print $Authcookivalue;
//print "-------";
//print $Authcookiname. '=' .$Authcookivalue;
//print "-------";
//print "$Authcookiname=$Authcookivalue";







//curl_setopt($ch, CURLOPT_COOKIE, $cookie );



ini_set('max_execution_time', 0);
   // EXECUTE:
   $result = curl_exec($curl);

/*
$f = fopen('request.txt', 'w');
curl_setopt($curl,CURLOPT_VERBOSE,true);
curl_setopt($curl,CURLOPT_HTTPAUTH ,$f);
*/


   if(!$result){die("Connection Failure");}
   curl_close($curl);


   return $result;
}

/////////////
function callAPINEW($method, $url, $data){
   $curl = curl_init();

   switch ($method){
      case "POST":
         curl_setopt($curl, CURLOPT_POST, 1);
         if ($data)
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
         break;
      case "PUT":
         curl_setopt($curl, CURLOPT_CUSTOMREQUEST, "PUT");
         if ($data)
            curl_setopt($curl, CURLOPT_POSTFIELDS, $data);                       
         break;
      default:
         if ($data)
            $url = sprintf("%s?%s", $url, http_build_query($data));
   }
   // OPTIONS:
   curl_setopt($curl, CURLOPT_URL, $url);
   curl_setopt($curl, CURLOPT_HTTPHEADER, array(
      'APIKEY: 111111111111111111111',
      'Content-Type: application/json',
   ));
   curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
   curl_setopt($curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);

curl_setopt($curl,  CURLOPT_HEADER,  1);


   // EXECUTE:
   $result = curl_exec($curl);


$header_size = curl_getinfo($curl, CURLINFO_HEADER_SIZE);
$header = substr($result, 0, $header_size);
$body = substr($result, $header_size);
//print "mamaliiiiiiiiiiiiiiiiiii";

//print $header;

//print "mamaliiiiiiiiiiiiiiiiiii";
//print $body;


//print "mamaliiiiiiiiiiiiiiiiiii";
////////

//curl_setopt($result,CURLOPT_COOKIEJAR,'cookie.txt');

   
// Matching the response to extract cookie value
preg_match_all('/^Set-Cookie:\s*([^;]*)/mi',
          $header,  $match_found);
   
$cookies = array();
foreach($match_found[1] as $item) {

    parse_str($item,  $cookie);
//print "cookiessssssssssssssssssss13";
//echo $cookie;



//print $item;
//print "cookiessssssssssssssssssss13";
//print $cookies;

//foreach($cookies as $flower){
 ///   echo $flower . "\n";
//}



    $cookies = array_merge($cookies,  $cookie);
}
   
// Printing cookie data
//print "cookiessssssssssssssssssssd";
//print_r( $cookies);

//print "cookiessssssssssssssssssssd";


//print $body; 


   if(!$result){die("Connection Failure");}
   curl_close($curl);

   //return $result;

//echo json_encode($cookies);

return array(
        'cookies' => json_encode($cookies),
        'body' => $body
    );


 //return $body;
}


?> 
 </body>
</html>
