#-*- mode: ruby -*-

aether_version = '1.13'
maven_version = '3.0.4'
wagon_version = '2.2'

jar 'org.sonatype.aether:aether-api', aether_version
jar 'org.sonatype.aether:aether-util', aether_version
jar 'org.sonatype.aether:aether-impl', aether_version
jar 'org.sonatype.aether:aether-connector-file', aether_version
jar 'org.sonatype.aether:aether-connector-asynchttpclient', aether_version
jar 'org.sonatype.aether:aether-connector-wagon', aether_version
jar 'org.apache.maven:maven-aether-provider', maven_version
jar 'org.apache.maven.wagon:wagon-file', wagon_version
jar 'org.apache.maven.wagon:wagon-http', wagon_version
#jar 'org.apache.maven.wagon:wagon-http-lightweight', wagon_version
jar 'org.apache.maven:maven-settings', maven_version
jar 'org.apache.maven:maven-settings-builder', maven_version

test_jar 'org.mockito:mockito-core', '1.9.5'
test_jar 'org.testng:testng', '6.8'

# overwrite via cli -Djruby.versions=1.6.7
# putting 1.5.6 at the end works around the problem of installing gems
# with "bad" timestamps
properties['jruby.versions'] = ['1.6.8','1.7.2','1.5.6'].join(',')
# overwrite via cli -Djruby.use18and19=false
properties['jruby.18and19'] = true

properties['jruby.plugins.version'] = '0.29.2'

plugin(:minitest) do |m|
  m.execute_goal(:spec)
end

profile 'run-its' do |r|
  r.plugin( :cucumber, '${jruby.plugins.version}' ) do |m|
    m.execute_goal(:test)
  end
end

plugin( :jar, '2.4' ).in_phase( 'prepare-package' ).execute_goal( :jar ).with :finalName => "${project.artifactId}", :outputDirectory => "${project.basedir}/lib"

plugin(:clean, '2.5' ).with :filesets => [ { :directory => './',
                                             :includes => [ 'Gemfile.lock', 
                                                            'lib/${project.artifactId}.jar' ] } ]

plugin( :compiler, '3.0' ).with( :target => '1.6', :source => '1.6' )

execute_in_phase( :initialize ) do
  pom = File.read( 'pom.xml' )
  if File.exists? '.pom.xml'
    dot_pom = File.read( '.pom.xml' )
    if pom != dot_pom
      File.open( 'pom.xml', 'w' ) { |f| f.puts dot_pom }
    end
  end
end

# vim: syntax=Ruby
