#Chen Gutman 205616147
#Avital Kahani 205688666

import json


class MatrixException(Exception):
    pass


class Matrix:
    def __init__(self, json_file=None):
        if json_file is None:
            json_file = input("Please enter a file name: ")
        try:
            with open(json_file) as f:
                # saves the data from file to Matrix object
                self.data = json.load(f)
            try:
                # saves the dictionary keys from data to Matrix object
                self.data_keys = self.data[0].keys()
            except IndexError:
                self.data_keys = []
        except:
            raise MatrixException("Cannot open file " + json_file + " no such file")

    def total(self, propertyname):
        # if no such key in dictionary raise an exception
        if propertyname not in self.data_keys:
            raise MatrixException("Cannot calculate total for key: " + propertyname + " No such key in dictionary")
        initial_flag = True
        for item in self.data:
            if initial_flag:
                _total = item[propertyname]
                initial_flag = False
            else:
                _total = _total + item[propertyname]
        return _total

    def average(self, propertyname):

        # if no such key in dictionary raise an exception
        if propertyname not in self.data_keys:
            raise MatrixException("Cannot calculate average for key: " + propertyname + " No such key in dictionary")
        _total = self.total(propertyname)
        # calculation of average is only on int
        if not isinstance(_total, int):
            raise MatrixException("Cannot calculate average for key: " + propertyname + " Values must be of type int")
        data_length = len(self.data)
        if data_length != 0:
            _avg = self.total(propertyname)/data_length
        else:
            # exception of division by zero
            raise MatrixException("Cannot calculate average for key: " + propertyname + ", cannot divide by zero")
        return _avg

    def min(self, propertyname):
        # if no such key in dictionary raise an exception
        if propertyname not in self.data_keys:
            raise MatrixException("Cannot find min value for key: " + propertyname + " No such key in dictionary")
        sorted_data = sorted(self.data, key=lambda k: k[propertyname])
        return sorted_data[0][propertyname]

    def max(self, propertyname):
        # if no such key in dictionary raise an exception
        if propertyname not in self.data_keys:
            raise MatrixException("Cannot find max value for key: " + propertyname + " No such key in dictionary")
        sorted_data = sorted(self.data, key=lambda k: k[propertyname], reverse=True)
        return sorted_data[0][propertyname]
