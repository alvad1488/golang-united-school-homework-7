package coverage

import (
	"errors"
	"os"
	"reflect"
	"sort"
	"strconv"
	"testing"
	"time"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

var errorWromgRows error = errors.New("Rows need to be the same length")

// WRITE YOUR CODE BELOW
func TestPerson_Sort (t *testing.T) {
	var tPeople, expData People
	tData := []struct{
		firstName string
		lastName  string
		birthDay  string
	}{
		{"Alex", "Vakhrushev", "1994-04-05"},
		{"Alex", "Algovich", "1994-04-05"},
		{"Viktor", "Gureev", "1994-04-05"},
		{"Alex", "Postrov", "1999-01-01"},
		{"Ivan", "Churov", "1999-01-02"},
		{"Goga", "Matushev", "2000-04-20"},
	}
	for _, per := range tData {
		date, ok := time.Parse("2006-01-02", per.birthDay)
		if ok != nil {
			t.Errorf("error when parse time: name: %s, date: %s", per.firstName, per.birthDay)
		} else {
			tPerson := Person {per.firstName, per.lastName, date}
			tPeople = append(tPeople, tPerson)
		}
	}

	expData = append(expData, tPeople[5])
	expData = append(expData, tPeople[4])
	expData = append(expData, tPeople[3])
	expData = append(expData, tPeople[1])
	expData = append(expData, tPeople[0])
	expData = append(expData, tPeople[2])

	sort.Sort(tPeople)

	if !reflect.DeepEqual(expData, tPeople) {
		t.Errorf("after sort got unexpected array: got %v, want %v", tPeople, expData)
	}

}

func TestMatrix_New (t *testing.T) {
	testCases := map[string]struct{
		input   string
		expData *Matrix
		expErr  error
		numErr  bool
	}{
		"positive_test":{input: "1 2\n3 4", expData: &Matrix{2,2,[]int{1,2,3,4}},expErr: nil, numErr: false},
		"wrong_rows_test": {input: "1 2\n34", expData: nil, expErr: errorWromgRows, numErr: false},
		"num_test":{input: "1 2\n3 a", expData: nil, expErr: strconv.ErrSyntax, numErr: true},
	}

	for name, cases := range testCases {
		t.Run(name, func(t *testing.T) {
			output, err := New(cases.input)
			if cases.expErr != nil {
				if cases.numErr {
					if err == nil {
						t.Errorf("%s:\n error didn`t got while it should", name)
					}
				} else {
					if  err.Error() != cases.expErr.Error() {
						t.Errorf("%s: wrong error returned: got %s, want %s", name, err.Error(), cases.expErr.Error())
					}
				}

				if output != nil {
					t.Errorf("%s: got struct when it shouldn`t", name)
				}
			} else {
				if !reflect.DeepEqual(output, cases.expData) {
					t.Errorf("%s: returned data is wrong: got %v, want %v", name, output, cases.expData)
				}
				if output.rows != cases.expData.rows {
					t.Errorf("%s: wrong number of rows: got %d, want %d", name, output.rows, cases.expData.rows)
				}
				if output.cols != cases.expData.cols {
					t.Errorf("%s: wrong number of cols: got %d, want %d", name, output.cols, cases.expData.cols)
				}
				if len(output.data) != len(cases.expData.data) {
					t.Errorf("%s: wrong length of data: got %d, want %d", name, len(output.data), len(cases.expData.data))
				}
			}
		})
	}

}

func TestMatrix_Rows (t *testing.T) {
	tString := "1 2 3\n4 5 6"
	expData := [][]int{{1, 2, 3},{4, 5, 6}}
	matrix, err := New(tString)

	if err == nil {
		data := matrix.Rows()
		if !reflect.DeepEqual(expData, data) {
			t.Errorf("func returned unexpected value: got %v, want %v", data, expData)
		}
	} else {
		t.Errorf("func return error while it shouldn`t")
	}
}

func TestMatrix_Cols (t *testing.T) {
	tString := "1 2 3\n4 5 6"
	expData := [][]int{{1, 4 },{2, 5}, {3, 6}}
	matrix, err := New(tString)

	if err == nil {
		data := matrix.Cols()
		if !reflect.DeepEqual(expData, data) {
			t.Errorf("func returned unexpected value: got %v, want %v", data, expData)
		}
	} else {
		t.Errorf("func return error when it shouldn`t")
	}
}

func TestMatrix_Set (t *testing.T) {
	testCases := map[string]struct{
		input   string
		sRow, sCol, sVal int
		expData bool
	}{
		"positive_test":{input: "1 2\n3 4", sRow: 0, sCol: 0, sVal: 999, expData: true},
		"more_rows_test": {input: "1 2\n3 4", sRow: 3, sCol: 0, sVal: 999, expData: false},
		"more_cols_test":{input: "1 2\n3 4", sRow: 0, sCol: 3, sVal: 999, expData: false},
		"equal_rows_test": {input: "1 2\n3 4", sRow: 2, sCol: 0, sVal: 999, expData: false},
		"equal_cols_test":{input: "1 2\n3 4", sRow: 0, sCol: 2, sVal: 999, expData: false},
	}

	for name, tt := range testCases {
		t.Run(name, func (t *testing.T) {
			tmpMatrix, err := New(tt.input)
			if err != nil {
				t.Errorf("%s\n: error return while it shouldn`t", name)
			} else {
				result := tmpMatrix.Set(tt.sRow, tt.sCol, tt.sVal)
				if !tt.expData && result || tt.expData && !result {
					t.Errorf("%s \n: func return unexpected result: got %v, want %v", name, result, tt.expData)
				}
			}
		})
	}
}

