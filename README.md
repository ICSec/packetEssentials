# packetEssentials
An essential set of modules for working with packets using the Scapy Library in Python.  Highly geared around 802.11, but still useful for Ethernet.

## Brief module descriptions
### chanFreq
Import for lib/chan_freq.py
This class is for channel/frequency specific tasks
* twoFour(val)
    ````python
    """Converts the 2.4GHz frequency to its decimal-based counterpart"""
    print(chanFreq.twoFour(2412))
    ````
* twoFourRev(val)
    ````python
    """Channel to Frequency converter for 2.4 GHz"""
    print(chanFreq.twoFourRev(4))
    ````
* fiveEight(val):
    ````python
    """Frequency to Channel converter for 5.8 GHz"""
    print(chanFreq.fiveEight(5200))
    ````
* fiveEightRev(val):
        ````python
        """Channel to Frequency converter for 5.8 GHz"""
        print(fiveEightrev(val):
        ````
    ````

### conv
Import for lib/converter.py
Class for simple conversions
* symString(packet, field)
    ````python
    """Shows the symblic string for a given field"""
    for i in range(200):
        p[0].FCfield = i
        print str(i) + '--' + conv.symString(p[0][Dot11], 'FCfield')
    ````

### drv
Import for lib/drivers.py
This class identifies the given offsets for drivers
* drivers(val)
    ````python
    """Returns the numeric driver offset for a given driver"""
    print drv.drivers('ath9k')
    ````

    ### hd
    Import for lib/handlers.py
    This class provides useful scapy frame handlers
    * crtlC()
        ````python
        """Handles what happens when crtl + c occurs
        Tries to deal with unexpected situations in which the collected lists are at
        risk of being lost
        """
        ````
    * metaDisplay(orderHigh = True):
        ````python
        """Returns self.metaCounts and self.metaSums as sorted lists
        The default is to return based on the value order of highest to lowest
        This is useful with regards to 802.11 in general.

        If a NIC is in range:
            - The RSSI for a given frame, at a particular point in space,
            relative to the location of the device in earshot can be considered
            the relative volume of the conversation.

            - The quantity of frames can be considered a metric of how chatty a
            given NIC is.

            - The sum of bytes transferred can be a metric in ratio to quantity,
             and other such things.  Logarithmic graphing helps in this respect.
        """
        ````
    * mpTraffic(macX, macY, verbose = False):
        ````python
        """Packet handler to follow a given pair of MAC addresses
        Uses macPair as a boolean wrapper to determine if both MACs were seen
        """
        ````
    * mpTrafficCap(macX, macY, q, verbose = False):
        ````python
        """Packet handler to follow a given pair of MAC addresses
        Captures self.mpTrafficList
        """
        ````
    * solo(macX, verbose = False):
        ````python
        """Packet handler to follow a given pair of MAC addresses"""
        ````
    * soloCap(macX, q, verbose = False):
        ````python
        """Packet handler to follow a given pair of MAC addresses
        Captures self.soloList
        """
        ````

### sType
Import for lib/subtypes.py
This class is for naming of subtypes where symStrings doesn't work
* mgmtSubtype
    ````python
    """Returns Management Frame Subtypes"""
    print sType.mgmtSubtype(5)
    ````
* ctrlSubtype
    ````python
    """Returns Control Frame Subtypes"""
    print sType.ctrlSubtype(11)
    ````
* dataSubtype
    ````python
    """Returns Data Frame Subtypes"""
    print sType.dataSubtype(8)
    ````


    ### pt
    Import for lib/utils.py
    Class to deal with packet specific options
    * byteRip
        ````python
        """Take a packet and grab a grouping of bytes, based on what you want

        byteRip can accept a scapy object or a scapy object in str() format
        Allowing byteRip to accept str() format allows for byte insertion

        Example of scapy object definition:
          - stream = Dot11WEP()

        Example of scapy object in str() format
          - stream = str(Dot11WEP())

        chop is the concept of removing the qty based upon the order
        compress is the concept of removing unwanted spaces
        order is concept of give me first <qty> bytes or gives me last <qty> bytes
        output deals with how the user wishes the stream to be returned
        qty is how many bytes to remove
        """
        ````
    * endSwap
        ````python
        """Takes an object and reverse Endians the bytes

        Useful for crc32 within 802.11:
        Autodetection logic built in for the following situations:
        Will take the stryng '0xaabbcc' and return string '0xccbbaa'
        Will take the integer 12345 and return integer 14640
        Will take the bytestream string of 'aabbcc' and return string 'ccbbaa'
        """
        print pt.endSwap('0xaabbcc')
        print pt.endSwap(12345)
        print pt.endSwap('aabbcc')
        ````
    * fcsGen
        ````python
        """Return the FCS for a given frame

        MODIFYING THIS PROBABLY BREAKS OTHER THINGS

        Where objFrame is the frame
        x = hexstr(objFrame, onlyhex = 1).replace(' ', '').lower()
        crc32(binascii.unhexlify(x.replace(' ', '')))

        """
        ````
    * macFilter(mac, pkt):
        ````python
        """ Combo whitelist and blacklist for given MAC address """
        ````
    * macPair(macX, macY, pkt):
        ````python
        """Pair up the MAC addresses, and follow them

        macX is weighted before macY, allowing the user to have a ranked format
        For fastest results, use macX as the quietest MAC
        """
        ````
    * symStryngs(self, scpObj, fld, maxInt = 254):
        ````python
        """Iterator to show the available opcodes for a given scapy object
        Returns a list object by default of 0-253 for the opcode
        """
        ````

## Uninstantiated modules
* lib/nic.py
  * Generates a tap interface
  ````python
  """Generates a tap interface
  By default, will create tap0 unless an integer parameter is added to Tap()
  """
  ## Create interface of tap3
  import packetEssentials as PE
  nTap = PE.lib.nic.Tap(3)
  ````
* lib/unifier.py
  * A singular point of contact for tracking purposes
  * Useful for passing around a Class with its associated objects
  ````python
  ## Keep track of wlan0mon using ath9k
  import packetEssentials as PE
  nUni = PE.lib.unifier.Unify('wlan0mon')
  print nUni.offset
  print nUni.nic
  ````
