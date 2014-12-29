# chef-ebs

This is a cookbook that makes it easy to create/attach EBS volumes, and create
filesystems and RAID arrays on them.


## Usage

### RAID Array Creation

Add `recipe[ebs]` to your run list, and configure these attributes:

Create a RAID 10 across four 10GB volumes each with 2000 provisioned iops, make it an lvm logical volume, format it with XFS, and mount it on
`/data`.

```ruby
{
  :ebs => {
    :raids => {
      '/dev/md0' => {
        :num_disks => 4,
        :disk_size => 10,
        :piops => 2000,
        :raid_level => 10,
        :fstype => 'xfs',
        :mount_point => '/data',
        :uselvm => true,
      }
    }
  }
}
```

### Use Existing Volumes for RAID Array

Add `recipe[persistent]` to your run list, and configure these attributes:

Create a RAID 10 across the volumes specified in the `persistent_volumes` array, do not use LVM,  format it with XFS, and mount it on `/data`.

```ruby
{
  :ebs => {
    :raids => {
      '/dev/md0' => {
        :raid_level => 10,
        :fstype => 'xfs',
        :mount_point => '/data',
        :uselvm => false,
        :persistent_volumes => [
          "vol-xxxxxxxx",
          "vol-xxxxxxxx",
          "vol-xxxxxxxx",
          "vol-xxxxxxxx"
        ]
      }
    }
  }
}
```

### EBS Volume Creation using gp2 (basic ssd)

Create a 10GB volume using the new `gp2` ssd, format it with XFS, and mount it on `/data` with `noatime` as an option.

```ruby
{
  :ebs => {
    :volumes => {
      '/data' => {
        :size => 10,
        :volume_type => 'gp2',
        :fstype => 'xfs',
        :mount_options => 'noatime'
      }
    }
  }
}
```

### EBS Volume Creation using standard (standard spining magnets)

Create a 10GB volume using the `standard` magnetic disk, format it with XFS, and mount it on `/data` with `noatime` as an option.

```ruby
{
  :ebs => {
    :volumes => {
      '/data' => {
        :size => 10,
        :volume_type => 'standard',
        :fstype => 'xfs',
        :mount_options => 'noatime'
      }
    }
  }
}
```

### EBS Volume Creation using provisioned iops ssd

Create a 10GB volume with 1000 provisioned iops, format it with XFS, and mount it on `/data` with `noatime,nobootwait` as an option.

```ruby
{
  :ebs => {
    :volumes => {
      '/data' => {
        :size => 10,
        :volume_type => 'io1',
        :piops => 1000,
        :fstype => 'xfs',
        :mount_options => 'noatime,nobootwait'
      }
    }
  }
}
```

`volume_type is optional if you specify `piops` to make it backwards compatible with version 0.3.6 and earlier. May be Depreciated in future so you should specify it
`mount_options` are optional and will default to `noatime,nobootwait` on all platforms except Amazon linux, where they will default to `noatime`.

## Credentials

Expects a `credentials` databag with an `aws` item that contains `aws_access_key_id` and `aws_secret_access_key`.

You can override the databag and item names with `node[:ebs][:creds][:databag]`, and `node[:ebs][:creds][:item]`, but the key names are static.

## Requirements

- [Opscode AWS Cookbook](https://github.com/opscode-cookbooks/aws)

## Acknowledgments

This code was originally forked from the Scalarium [ebs cookbook][1] which has since been taken over by [AWS Opsworks][2].

[1]: https://github.com/aws/opsworks-cookbooks/tree/master-chef-11.4/ebs
[2]: http://aws.amazon.com/opsworks
