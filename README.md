# 7a
Handpicked fresh ingredients, thoughtfully combined and expertly prepared, come together to create a harmonious fusion of taste and aroma. Whether you're a seasoned chef or a home-cooking enthusiast, this recipe offers an approachable yet elevated culinary experience.
<?php 
	$dbname = "guestbook";
	$servername = "localhost";
	$username = "root";
	$password = "";

	$name = isset($_POST['name'])?$_POST['name']:'';
	$add1 = isset($_POST['add1'])?$_POST['add1']:'';
	$add2 = isset($_POST['add2'])?$_POST['add2']:'';
	$email = isset($_POST['email'])?$_POST['email']:'';

	$con = mysqli_connect($servername,$username,$password,$dbname);

	$query = "INSERT into guestinfo values('','$name','$add1','$add2','$email')";
	if(isset($_POST['submit'])) {

		if(strlen($name == '' || $add1 == '' || $email == '')) {
			echo "Sorry, feilds cannot be left blank";
		} else {
			$result = mysqli_query($con,$query);
			if($result) {
				echo "Details successfully inserted";
			} else {
				echo "Sorry, something went wrong. Could not insert details ".mysqli_error($con);
			}	
		}
			
	}
	
?>

<?php 	

	if(isset($_POST['search'])) {
		$key = isset($_POST['keyword'])?$_POST['keyword']:'';
		$find_name = mysqli_query($con, "SELECT name,add1,add2,email FROM guestinfo WHERE name LIKE '%$key%' ");
		$count = mysqli_num_rows($find_name);

		if($count == 0) {
			echo "Sorry, No such name exist in the database..";
		} else if($count == 1){
			echo "<table border='1'><tr><th>Name</th><th>Address1</th><th>Email Address</th></tr>";
			while ($row = mysqli_fetch_assoc($find_name)) {
				echo "<tr>
					<td>$row[name]</td>
					<td>$row[add1]</td>
					<td>$row[email]</td>
					</tr></table>";
			}			
		}
	}

?>

<!DOCTYPE html>
<html>
<head>
	<title>My GuestBook</title>

	<!--  The styling goes here...  -->
	<style type="text/css">
		body{
			margin-left: 25%; 
		}
		input {
			width: 40%;
			padding: 12px;
			margin: 7px 1px;
		}
		textarea {
			width: 40%;
			padding: 12px;
			margin: 7px 1px;

		}
		input[type=submit] {
			width: 20%;
			margin-left: 10%;
		}
	</style>
	<!--  End of styling  -->

</head>
<body>
<h2>Guest Book Applicatioin</h2>
<form action="guestbook.php" method="post">	
	<input type="text" name="name" placeholder ="Please enter your name"><br>
	<textarea name="add1" placeholder="Enter Address1"></textarea> <br>
	<textarea name="add2" placeholder="Enter Address2"></textarea><br>
	<input type="text" name="email" placeholder="Enter email address"><br>
	<input type="submit" name="submit" value="Send Info!!">
</form>

<br><br>

<h3>Student Name Search</h3>

<form action="guestbook.php" method="POST">
	<input type="text" name="keyword" placeholder="Please enter the name of student"><br>
	<input type="submit" name="search" value="Search Name">
</form>


</body>
</html>
