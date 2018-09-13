<?php

Class MySqliData
{
    private $link = '';
    public $table = '';
    public $data = [];
    public $query = '';

    public function __construct()
    {
        $this->link = mysqli_connect('localhost', 'root', '', 'convert_lang');
		mysqli_set_charset($this->link,'utf8');
    }

    public function login($email, $password)
    {
        $success = 0;
        try {
            $sql = "select user_id from users where email=? && password=?";

            if ($res = mysqli_prepare($this->link, $sql)) {
                mysqli_stmt_bind_param($res, 'ss', $email, md5($password));
                mysqli_stmt_execute($res);
                mysqli_stmt_bind_result($res, $user_id);
                mysqli_stmt_fetch($res);
                mysqli_stmt_close($res);
                if ($user_id) $success = 1;
            }

        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function changePassword($email, $oldpassword, $newpassword)
    {
        $success = 0;
        try {
            $sql = "select * from users where email='$email' and password=md5('$oldpassword')";
            $res = mysqli_query($this->link, $sql);
            if (mysqli_num_rows($res) > 0) {
                $sql = "update users set password=md5('$newpassword') where email='$email' and password=md5('$oldpassword')";

                if (mysqli_query($this->link, $sql)) {
                    $success = 1;
                }

            }

        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function getRecord($order = null)
    {

        try {
            $str = '';
            if ($order) $str = " ORDER BY $order";
            $sql = "select * from $this->table" . $str;
            $res = mysqli_query($this->link, $sql);
            if ($res) {
                while ($row = $res->fetch_array(MYSQLI_ASSOC)) {
                    $results[] = $row;
                }
            } else {
                $results = null;

            }

        } catch (Exception $ex) {
            $results = null;
        }
        return (!empty($results)) ? $results : null;

    }

    public function getRecordById($id, $key = 'id', $order = null)
    {
        $str = '';
        if ($order) $str = " ORDER BY $order";
        try {
            $sql = "select * from $this->table where $key='" . $id . "'" . $str;

            $res = mysqli_query($this->link, $sql);
            if ($res) {
                while ($row = $res->fetch_array(MYSQLI_ASSOC)) {
                    $results[] = $row;
                }
            } else {
                $results = null;

            }

        } catch (Exception $ex) {
            $results = null;
        }
        return (!empty($results)) ? $results : null;

    }

    public function saveRecord()
    {
        $keys = implode(',', array_keys($this->data));
        $vals = implode("','", $this->data);
        $success = 0;
        try {

            $sql = "insert into $this->table ($keys) values ('$vals')";
            if (mysqli_query($this->link, $sql)) {
                $success = mysqli_insert_id($this->link);
            }
        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function deleteRecord($id, $key = 'id')
    {
        $success = 0;
        try {
            $sql = "delete from $this->table where $key=" . $id;
            if (mysqli_query($this->link, $sql)) {
                $success = 1;
            }
        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function updateRecord($id)
    {
        $str = [];
        foreach ($this->data as $key => $val) {
            $str[] = $key . '=' . "'" . $val . "'";
        }
        $str = implode(',', $str);
        $success = 0;
        try {
            $sql = "update $this->table set $str where id=$id ";
            if (mysqli_query($this->link, $sql)) {
                $success = 1;
            }
        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function saveOrUpdateValue()
    {
        $str = '';
        if ($this->data['line_value_en'] == '') {
            $str = "line_value_sv='" . $this->data['line_value_sv'] . "'";
        } elseif ($this->data['line_value_sv'] == '') {
            $str = "line_value_en='" . $this->data['line_value_en'] . "'";
        }
        $file_id = $this->data['file_id'];
        $sql = "select id as ct from $this->table where file_id=$file_id && line_key='" . $this->data['line_key'] . "'";
        $res = mysqli_query($this->link, $sql);
        if (mysqli_num_rows($res)) {
            $sql = "update $this->table set $str where file_id=$file_id && line_key='" . $this->data['line_key'] . "'";
            mysqli_query($this->link, $sql);
        } else {
            $this->saveRecord();
        }
    }

    public function addNewColumnToTable($columnName)
    {
        $success = 0;
        try {
            $sql = "ALTER TABLE $this->table ADD $columnName TEXT DEFAULT NULL";

            if (mysqli_query($this->link, $sql)) {
                $success = 1;
            }
        } catch (Exception $ex) {
            $success = 0;
        }
        return $success;
    }

    public function executeQuery()
    {
        try {
            $res = mysqli_query($this->link, $this->query);
            if ($res) {
                while ($row = $res->fetch_array(MYSQLI_ASSOC)) {
                    $results[] = $row;
                }
            } else {
                $results = null;

            }

        } catch (Exception $ex) {
            $results = null;
        }
        return (!empty($results)) ? $results : null;
    }

    function strafter($string, $substring)
    {
        $pos = strpos($string, $substring);
        if ($pos === false)
            return $string;
        else
            return (substr($string, $pos + strlen($substring)));
    }
} 
echo "git2";
echo "git1";
echo "conflict";
?>