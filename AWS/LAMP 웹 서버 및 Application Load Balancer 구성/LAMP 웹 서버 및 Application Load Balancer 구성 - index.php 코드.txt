<?php include "dbinfo.inc"; ?>
<html>
<body>
<h1>Instance data</h1>
<?php

  echo "<table>";
  echo "<tr><th>Data</th><th>Value</th></tr>";

  $urlRoot="http://169.254.169.254/latest/meta-data/";

  echo "<tr><td>Instance ID</td><td><i>" . file_get_contents($urlRoot . 'instance-id') . "</i></td><tr>";
  echo "<tr><td>Private IP Address</td><td><i>" . file_get_contents($urlRoot . 'local-ipv4') . "</i></td><tr>";
  echo "<tr><td>Public IP Address</td><td><i>" . file_get_contents($urlRoot . 'public-ipv4') . "</i></td><tr>";
  echo "<tr><td>Availability Zone</td><td><i>" . file_get_contents($urlRoot . 'placement/availability-zone') . "</i></td><tr>";

  echo "</table>";

?>
<h1>RDS Practice</h1>
<?php

  /* Connect to MySQL and select the database. */
  $connection = mysqli_connect(DB_SERVER, DB_USERNAME, DB_PASSWORD);

  if (mysqli_connect_errno()) echo "Failed to connect to MySQL: " . mysqli_connect_error();

  $database = mysqli_select_db($connection, DB_DATABASE);

?>

<!-- Display table data. -->
<table border="1" cellpadding="2" cellspacing="2">
  <tr>
    <td>ID</td>
    <td>NAME</td>
    <td>ADDRESS</td>
  </tr>

<?php

$result = mysqli_query($connection, "SELECT * FROM SAMPLE");

while($query_data = mysqli_fetch_row($result)) {
  echo "<tr>";
  echo "<td>",$query_data[0], "</td>",
       "<td>",$query_data[1], "</td>",
       "<td>",$query_data[2], "</td>";
  echo "</tr>";
}
?>

</table>

<!-- Clean up. -->
<?php

  mysqli_free_result($result);
  mysqli_close($connection);

?>

</body>
</html>