# id-sorting1
In order to do sorting on id in the attendence section of moodle course, we have to do some changes in our .php files.
 1. **moodle\mod\attendance\locallib.php**
 2. **moodle\mod\attendance\classes\output\renderer.php**
 3. **moodle\mod\attendance\classes\structure.php**
 
 1.  ***\moodle\mod\attendance\locallib.php***
### Original:
       define('ATT_SORT_DEFAULT', 0);\
      define('ATT_SORT_LASTNAME', 1);\
      define('ATT_SORT_FIRSTNAME', 2);
### Replace with:
      define('ATT_SORT_DEFAULT', 3);\
     define('ATT_SORT_IDNUMBER', 3);\
     define('ATT_SORT_LASTNAME', 1);\
     define('ATT_SORT_FIRSTNAME', 2);
     
   2. ## ***\moodle\mod\attendance\classes\output\renderer.php***
  
### Original:
if ($data->pageparams->sort == ATT_SORT_LASTNAME) {\
            $url->param('sort', ATT_SORT_FIRSTNAME);\
            $firstname = html_writer::link($url, get_string('firstname'));\
            $lastname = get_string('lastname');\
        } else if ($data->pageparams->sort == ATT_SORT_FIRSTNAME) {\
            $firstname = get_string('firstname');\
            $url->param('sort', ATT_SORT_LASTNAME);\
            $lastname = html_writer::link($url, get_string('lastname'));\
        } else {\
            $firstname = html_writer::link($data->url(array('sort' => ATT_SORT_FIRSTNAME)), get_string('firstname'));\
            $lastname = html_writer::link($data->url(array('sort' => ATT_SORT_LASTNAME)), get_string('lastname'));\
        }
        if ($CFG->fullnamedisplay == 'lastname firstname') {\
            $fullnamehead = "$lastname / $firstname";        } else {\
            $fullnamehead = "$firstname / $lastname ";\
        }

### Replace with:
        if ($data->pageparams->sort == ATT_SORT_IDNUMBER) {\
            $url->param('sort', ATT_SORT_FIRSTNAME);\
            $firstname = html_writer::link($url, get_string('firstname'));\
            $url->param('sort', ATT_SORT_LASTNAME);\
            $lastname = html_writer::link($url, get_string('lastname'));\
            $idnumber = get_string('idnumber');\
        } else if ($data->pageparams->sort == ATT_SORT_LASTNAME) {\
            $lastname = get_string('lastname');\
            $url->param('sort', ATT_SORT_IDNUMBER);\
            $idnumber = html_writer::link($url, get_string('idnumber'));\
            $url->param('sort', ATT_SORT_FIRSTNAME);\
            $firstname = html_writer::link($url, get_string('firstname'));\
        } else if ($data->pageparams->sort == ATT_SORT_FIRSTNAME) {\
            $firstname = get_string('firstname');\
            $url->param('sort', ATT_SORT_LASTNAME);\
            $lastname = html_writer::link($url, get_string('lastname'));\
            $url->param('sort', ATT_SORT_IDNUMBER);\
            $idnumber = html_writer::link($url, get_string('idnumber'));\
        } else {\
    $idnumber = html_writer::link($data->url(array('sort' => ATT_SORT_IDNUMBER)), get_string('idnumber'));\
            $firstname = html_writer::link($data->url(array('sort' => ATT_SORT_FIRSTNAME)), get_string('firstname'));\
            $lastname = html_writer::link($data->url(array('sort' => ATT_SORT_LASTNAME)), get_string('lastname'));\
        }

        if ($CFG->fullnamedisplay == 'idnumber firstname lastname') {\
                    $fullnamehead = "$idnumber / $firstname / $lastname";\
        } else if ($CFG->fullnamedisplay == 'firstname lastname idnumber') {\
            $fullnamehead = "$firstname / $lastname / $idnumber";\
        }  else {\
            $fullnamehead = "$firstname / $lastname / $idnumber";\
        }

  3. ## ***\moodle\mod\attendance\classes\structure.php***

### Original:
if (empty($this->pageparams->sort)) {\
            $this->pageparams->sort = ATT_SORT_DEFAULT;\
        }\
        if ($this->pageparams->sort == ATT_SORT_FIRSTNAME) {\
            $orderby = $DB->sql_fullname('u.firstname', 'u.lastname') . ', u.id';\
        } else if ($this->pageparams->sort == ATT_SORT_LASTNAME) {\
            $orderby = 'u.lastname, u.firstname, u.id';
        } else {\
            list($orderby, $sortparams) = users_order_by_sql('u');\
        }

### Replace with:
if (empty($this->pageparams->sort)) {\
            $this->pageparams->sort = ATT_SORT_DEFAULT;\
        }\
        if ($this->pageparams->sort == ATT_SORT_IDNUMBER) {\
            $orderby = $DB->sql_fullname('u.idnumber', 'u.lastname') . ', u.id';\
        }else if ($this->pageparams->sort == ATT_SORT_FIRSTNAME) {\
            $orderby = $DB->sql_fullname('u.firstname', 'u.idnumber') . ', u.id';\
        } else if ($this->pageparams->sort == ATT_SORT_LASTNAME) {\
            $orderby = 'u.lastname, u.idnumber, u.id';\
        } else {\
            list($orderby, $sortparams) = users_order_by_sql('u');  }
            
            
