<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>Animestan | دنیای انیمه</title>
    <style>
        body { font-family: Tahoma, sans-serif; background-color: #f4f4f4; text-align: center; padding: 50px; }
        .container { background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); max-width: 500px; margin: auto; }
        h1 { color: #e91e63; }
        input[type="file"] { margin: 20px 0; }
        input[type="submit"] { background: #e91e63; color: white; border: none; padding: 10px 20px; cursor: pointer; border-radius: 5px; }
        .file-list { margin-top: 30px; text-align: right; }
        .file-item { padding: 10px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; }
        a { text-decoration: none; color: #2196f3; }
    </style>
</head>
<body>

<div class="container">
    <h1>Animestan 🌸</h1>
    <p>آپلود و دانلود فایل‌های PDF انیمه</p>

    <!-- فرم آپلود -->
    <form action="" method="post" enctype="multipart/form-data">
        <input type="file" name="pdfFile" accept="application/pdf" required><br>
        <input type="submit" name="submit" value="آپلود در انیمستان">
    </form>

    <?php
    $targetDir = "uploads/";
    
    // اگر پوشه آپلود وجود ندارد، ساخته شود
    if (!file_exists($targetDir)) {
        mkdir($targetDir, 0777, true);
    }

    // پردازش آپلود
    if (isset($_POST["submit"])) {
        $fileName = basename($_FILES["pdfFile"]["name"]);
        $targetFilePath = $targetDir . $fileName;
        $fileType = pathinfo($targetFilePath, PATHINFO_EXTENSION);

        if ($fileType == "pdf") {
            if (move_uploaded_file($_FILES["pdfFile"]["tmp_name"], $targetFilePath)) {
                echo "<p style='color:green;'>✅ فایل با موفقیت آپلود شد!</p>";
            } else {
                echo "<p style='color:red;'>❌ خطا در آپلود فایل.</p>";
            }
        } else {
            echo "<p style='color:red;'>⚠️ فقط فایل PDF مجاز است!</p>";
        }
    }
    ?>

    <div class="file-list">
        <h3>📂 لیست فایل‌های موجود:</h3>
        <?php
        if (is_dir($targetDir)) {
            $files = array_diff(scandir($targetDir), array('.', '..'));
            if (count($files) > 0) {
                foreach ($files as $file) {
                    echo "<div class='file-item'>
                            <span>$file</span>
                            <a href='$targetDir$file' download>دانلود</a>
                          </div>";
                }
            } else {
                echo "<p>هنوز فایلی آپلود نشده است.</p>";
            }
        }
        ?>
    </div>
</div>

</body>
</html>
