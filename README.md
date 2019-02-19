# Php
Sql and php to delete data from a certain data base
<?php
require_once "pdo.php";
session_start();
if (! isset($_SESSION['name'])){
  die("ACCESS DENIED");
}
if ( isset($_POST['delete']) && isset($_POST['car_id']) ) {
    $sql = "DELETE FROM autos WHERE car_id = :zip";
    $stmt = $pdo->prepare($sql);
    $stmt->execute(array(':zip' => $_POST['car_id']));
    $_SESSION['success'] = 'Record deleted';
    header( 'Location: index.php' ) ;
    return;
}

// Guardian: Make sure that car_id is present
if ( ! isset($_GET['car_id']) ) {
  $_SESSION['error'] = "Missing car_id";
  header('Location: index.php');
  return;
}

$stmt = $pdo->prepare("SELECT make, car_id FROM autos where car_id = :xyz");
$stmt->execute(array(":xyz" => $_GET['car_id']));
$row = $stmt->fetch(PDO::FETCH_ASSOC);
if ( $row === false ) {
    $_SESSION['error'] = 'Bad value for car_id';
    header( 'Location: index.php' ) ;
    return;
}

?>
<p>Confirm: Deleting <?= htmlentities($row['make']) ?></p>

<form method="post">
<input type="hidden" name="car_id" value="<?= $row['car_id'] ?>">
<input type="submit" value="Delete" name="delete">
<a href="index.php">Cancel</a>
</form>
