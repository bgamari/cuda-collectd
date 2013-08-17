#!/usr/bin/env python

import collectd
import subprocess
import xml.etree.ElementTree as ET

def read(data=None):
        vl = collectd.Values(type='gauge')
        vl.plugin = 'cuda'

        out = subprocess.check_output(['nvidia-smi', '-q', '-x'])
        root = ET.fromstring(out)

        for gpu in root.iter('gpu'):
                vl.plugin_instance = 'cuda-%s' % (gpu.attrib['id'])

                vl.dispatch(type='fanspeed',
                            values=float(gpu.find('fan_speed').text.split()[0]))

                vl.display(type='temperature',
                           values=float(gpu.find('temperature/gpu_temp').text.split()[0]))

                vl.dispatch(type='memory', type_instance='used',
                            values=float(1e6 * gpu.find('memory_usage/used').text.split()[0]))

collectd.register_read(read)

